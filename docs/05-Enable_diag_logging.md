# Enable diagnostics logging for apps in Azure App Service

**Azure** provides built-in diagnostics to assist with debugging an App Service app. In this lesson, you will learn how to enable diagnostic logging and add instrumentation to your application, as well as how to access the information logged by Azure.

Below is a table showing the types of logging, the platforms supported, and where the logs can be stored and located for accessing the information.

test23

## Enable application logging (Windows)

2. Select **On** for either 1

3. The **Filesystem** option is for temporary debugging purposes, and turns itself off in 12 hours. The **Blob** option is for long-term logging, and needs a blob storage container to write logs to. The Blob option also includes additional information in the log messages, such as the ID of the origin VM instance of the log message (`InstanceId`), thread ID (`Tid`), and a more granular timestamp (`EventTickCount`).

4. You can also set the **Level** of details included in the log as shown in the table below.


5. When finished, select **Save**.

## Enable application logging (Linux/Container)

1. In **Application logging**, select **File System**.

2. In **Quota (MB)**, specify the disk quota for the application logs. In **Retention Period (Days)**, set the number of days the logs should be retained.

3. When finished, select **Save**.

## Enable web server logging

1. For **Web server logging**, select **Storage** to store logs on blob storage, or **File System** to store logs on the App Service file system.

2. In **Retention Period (Days)**, set the number of days the logs should be retained.

3. When finished, select **Save**.

## Add log messages in code

In your application code, you use the usual logging facilities to send log messages to the application logs. For example:

* ASP.NET applications can use the `System.Diagnostics.Trace` class to log information to the application diagnostics log. For example:
 
    ```csharp
    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");
    ```

* By default, ASP.NET Core uses the `Microsoft.Extensions.Logging.AzureAppServices` logging provider.

## Stream logs

Before you stream logs in real time, enable the log type that you want. Any information written to files ending in .txt, .log, or .htm that are stored in the `/LogFiles` directory (`d:/home/logfiles`) is streamed by App Service.

>**Note**: Some types of logging buffer write to the log file, which can result in out of order events in the stream. For example, an application log entry that occurs when a user visits a page may be displayed in the stream before the corresponding HTTP log entry for the page request.

* Azure Portal - To stream logs in the Azure portal, navigate to your app and select **Log stream**.

* Azure CLI - To stream logs live in Cloud Shell, use the following command:
    ```bash
    az webapp log tail --name appname --resource-group myResourceGroup
    ```

* Local console - To stream logs in the local console, install Azure CLI and sign in to your account. Once signed in, follow the instructions for Azure CLI above.

## Access log files

If you configure the Azure Storage blobs option for a log type, you need a client tool that works with Azure Storage. 

For logs stored in the App Service file system, the easiest way is to download the ZIP file in the browser at:

* Linux/container apps: `https://<app-name>.scm.azurewebsites.net/api/logs/docker/zip`
* Windows apps: `https://<app-name>.scm.azurewebsites.net/api/dump`

For Linux/container apps, the ZIP file contains console output logs for both the docker host and the docker container. For a scaled-out app, the ZIP file contains one set of logs for each instance. In the App Service file system, these log files are the contents of the */home/LogFiles* directory.

