---
title: Profile live web apps on Azure with Application Insights Profiler | Microsoft Docs
description: Identify the hot path in your web server code with a low-footprint profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin

---
# Profile live Azure web apps with Application Insights

*This feature of Application Insights is generally available for Azure App Service and is in preview for Azure compute resources.*

Find out how much time is spent in each method of your live web application when using [Application Insights](app-insights-overview.md). The Application Insights profiling tool shows detailed profiles of live requests that were served by your app, and highlights the *hot path* that uses the most time. Requests with different response times are profiled on a sampling basis. Overhead to the application is minimized using various techniques.

The profiler currently works for ASP.NET and ASP.NET Core web apps running on Azure App Service. The **Basic** service tier or higher is required to use the Profiler.

## <a id="installation"></a> Enable the profiler for App Services Web App
If you already have the application published to an App Services, but have not done anything in the source code to use Application Insights, navigate to the App Services pane on the Azure portal, go to **Monitoring | Application Insights**, follow the instructions on the pane to create a new resource or select an existing Application Insights resource to monitor your Web App.

![Enable App Insights on App Services portal][appinsights-in-appservices]

If you have access to your project source code, [install Application Insights](app-insights-asp-net.md). If it's already installed, make sure you have the latest version. To check for the latest version, in Solution Explorer, right-click your project, and then select **Manage NuGet packages** > **Updates** > **Update all packages**. Then, deploy your app.

ASP.NET Core applications require the installation of the Microsoft.ApplicationInsights.AspNetCore NuGet package 2.1.0-beta6 or later to work with the profiler. As of June 27, 2017, earlier versions are not supported.

In [the Azure portal](https://portal.azure.com), open the Application Insights resource for your web app. Select **Performance** > **Enable Application Insights Profiler**.

![Select the Enable profiler banner][enable-profiler-banner]

Alternatively, you can select **Profiler** configuration to view the status and enable or disable the profiler.

![Under Performance, select Profiler configuration][performance-blade]

Web apps that are configured with Application Insights are listed in the **Profiler** configuration pane. If you followed the steps above, the profiler agent should be installed already. Select **Enable Profiler** in the **Profiler** configuration pane.

Follow instructions to install the profiler agent, if needed. If no web apps have been configured with Application Insights, select **Add Linked Apps**.

![Configure pane options][linked app services]

Unlike web apps that are hosted through App Service plans, applications that are hosted in Azure compute resources (for example, Azure Virtual Machines, virtual machine scale sets, Azure Service Fabric, or Azure Cloud Services) are not directly managed by Azure. In this case, there's no web app to link to. Instead of linking to an app, select the **Enable Profiler** button.

### Enable the profiler for Azure compute resources (preview)

Get information about a [preview version of the profiler for Azure compute resources](https://go.microsoft.com/fwlink/?linkid=848155).

## View profiler data

**Make sure your application is receiving traffic.** If you are doing an experiment, you can generate requests to your Web App using [Application Insights Performance Testing](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). If you newly enabled the Profiler, you can run a short load test for about 15 minutes, which should generate profiler traces. If you have had the Profiler enabled for a while already, keep in mind that the Profiler runs randomly two times every hour and for a duration of two minutes each time it runs. We recommend first running the load test for one hour to make sure you get sample profiler traces.

Once your application receives some traffic, go to the **Performance** blade > **Take Actions** to view profiler traces. Select the **Profiler Traces** button.

![Application Insights Performance pane preview profiler traces][performance-blade-v2-examples]

Select a sample to show a code-level breakdown of time spent executing the request.

![Application Insights trace explorer][trace-explorer]

The trace explorer shows the following information:

* **Show Hot Path**: Opens the biggest leaf node, or at least something close. In most cases, this node
is adjacent to a performance bottleneck.
* **Label**: The name of the function or event. The tree shows a mix of code and events that occurred (like SQL and HTTP events). The top event represents the overall request duration.
* **Elapsed**: The time interval between the start of the operation and the end of the operation.
* **When**: When the function or event was running in relation to other functions.

## How to read performance data

The Microsoft service profiler uses a combination of sampling methods and instrumentation to analyze the performance of your application. When detailed collection is in progress, the service profiler samples the instruction pointer of each of the machine's CPUs every millisecond. Each sample captures the complete call stack of the thread that is currently executing. It gives detailed information about what that thread was doing, both at a high level and at a low level of abstraction. The service profiler also collects other events to track activity correlation and causality, including context switching events, Task Parallel Library (TPL) events, and thread pool events.

The call stack that's shown in the timeline view is the result of the sampling and instrumentation. Since each sample captures the complete call stack of the thread, it includes code from the Microsoft .NET Framework, and from other frameworks that you reference.

### <a id="jitnewobj"></a>Object allocation (clr!JIT\_New or clr!JIT\_Newarr1)
**clr!JIT\_New** and **clr!JIT\_Newarr1** are helper functions in the .NET Framework that allocate memory from a managed heap. **clr!JIT\_New** is invoked when an object is allocated. **clr!JIT\_Newarr1** is invoked when an object array is allocated. These two functions are typically fast, and take relatively small amounts of time. If you see **clr!JIT\_New** or **clr!JIT\_Newarr1** take a substantial amount of time in your timeline, it's an indication that the code might be allocating many objects and consuming significant amounts of memory.

### <a id="theprestub"></a>Loading code (clr!ThePreStub)
**clr!ThePreStub** is a helper function in the .NET Framework that prepares the code to execute for the first time. This typically includes, but is not limited to, just-in-time (JIT) compilation. For each C# method, **clr!ThePreStub** should be invoked at most once during the lifetime of a process.

If **clr!ThePreStub** takes a substantial amount of time for a request, this indicates that the request is the first one that executes that method. The time for the .NET Framework runtime to load the first method is significant. You might consider using a warmup process that executes that portion of the code before your users access it, or consider running Native Image Generator (ngen.exe) on your assemblies.

### <a id="lockcontention"></a>Lock contention (clr!JITutil\_MonContention or clr!JITutil\_MonEnterWorker)
**clr!JITutil\_MonContention** or **clr!JITutil\_MonEnterWorker** indicates that the current thread is waiting for a lock to be released. This typically shows up when executing a C# **LOCK** statement, when invoking the **Monitor.Enter** method, or when invoking a method with the **MethodImplOptions.Synchronized** attribute. Lock contention typically occurs when thread _A_ acquires a lock, and thread _B_ tries to acquire the same lock before thread _A_ releases it.

### <a id="ngencold"></a>Loading code ([COLD])
If the method name contains **[COLD]**, such as **mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined**, the .NET Framework runtime is executing code for the first time that is not optimized by <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profile-guided optimization</a>. For each method, it should show up at most once during the lifetime of the process.

If loading code takes a substantial amount of time for a request, this indicates that the request is the first one to execute the unoptimized portion of the method. Consider using a warmup process that executes that portion of the code before your users access it.

### <a id="httpclientsend"></a>Send HTTP request
Methods like **HttpClient.Send** indicate that the code is waiting for an HTTP request to be completed.

### <a id="sqlcommand"></a>Database operation
Methods like **SqlCommand.Execute** indicate that the code is waiting for a database operation to finish.

### <a id="await"></a>Waiting (AWAIT\_TIME)
**AWAIT\_TIME** indicates that the code is waiting for another task to finish. This typically happens with the C# **AWAIT** statement. When the code does a C# **AWAIT**, the thread unwinds and returns control to the thread pool, and there is no thread that is blocked waiting for the **AWAIT** to finish. However, logically, the thread that did the **AWAIT** is "blocked," and is waiting for the operation to finish. The **AWAIT\_TIME** statement indicates the blocked time waiting for the task to finish.

### <a id="block"></a>Blocked time
**BLOCKED_TIME** indicates that the code is waiting for another resource to be available. For example, it might be waiting for a synchronization object, for a thread to be available, or for a request to finish.

### <a id="cpu"></a>CPU time
The CPU is busy executing the instructions.

### <a id="disk"></a>Disk time
The application is performing disk operations.

### <a id="network"></a>Network time
The application is performing network operations.

### <a id="when"></a>When column
The **When** column is a visualization of how the INCLUSIVE samples collected for a node vary over time. The total range of the request is divided into 32 time buckets. The inclusive samples for that node are accumulated in those 32 buckets. Each bucket is represented as a bar. The height of the bar represents a scaled value. For nodes marked **CPU_TIME** or **BLOCKED_TIME**, or where there is an obvious relationship of consuming a resource (CPU, disk, thread), the bar represents consuming one of those resources for the period of time of that bucket. For these metrics, it's possible to get a value of greater than 100% by consuming multiple resources. For example, if on average you use two CPUs during an interval, you get 200%.

## Limitations

The default data retention is five days. The maximum data ingested per day is 10 GB.

There are no charges for using the profiler service. To use the profiler service, your web app must be hosted in at least the Basic tier of Azure App Service.

## Overhead and sampling algorithm

The profiler randomly runs two minutes every hour on each virtual machine that hosts the application that has the profiler enabled for capturing traces. When the profiler is running, it adds between 5% and 15% CPU overhead to the server.
The more servers that are available for hosting the application, the less impact the profiler has on the overall application performance. This is because the sampling algorithm results in the profiler running on only 5% of servers at any time. More servers are available to serve web requests to offset the server overhead caused by running the profiler.

## Disable the profiler
To stop or restart the profiler for an individual App Service instance, under **Web Jobs**, go to the App Service resource. To delete the profiler, go to **Extensions**.

![Disable profiler for a web jobs][disable-profiler-webjob]

We recommend that you have the profiler enabled on all your web apps to discover any performance issues as early as possible.

If you use WebDeploy to deploy changes to your web application, ensure that you exclude the App_Data folder from being deleted during deployment. Otherwise, the profiler extension's files are deleted the next time you deploy the web application to Azure.


## <a id="troubleshooting"></a>Troubleshooting

### Too many active profiling sessions

Currently, you can enable profiler on a maximum of four Azure web apps and deployment slots that are running in the same service plan. If the profiler web job is reporting too many active profiling sessions, move some web apps to a different service plan.

### How do I determine whether Application Insights Profiler is running?

The profiler runs as a continuous web job in the web app. You can open the web app resource in [the Azure portal](https://portal.azure.com). In the **WebJobs** pane, check the status of **ApplicationInsightsProfiler**. If it isn't running, open **Logs** to get more information.

### Why can't I find any stack examples, even though the profiler is running?

Here are a few things that you can check:

* Make sure that your web app service plan is Basic tier or above.
* Make sure that your web app has Application Insights SDK 2.2 Beta or later enabled.
* Make sure that your web app has the **APPINSIGHTS_INSTRUMENTATIONKEY** setting configured with the same instrumentation key that's used by the Application Insights SDK.
* Make sure that your web app is running on .NET Framework 4.6.
* If your web app is an ASP.NET Core application, check [the required dependencies](#aspnetcore).

After the profiler is started, there is a short warmup period during which the profiler actively collects several performance traces. After that, the profiler collects performance traces for two minutes every hour.

### I was using Azure Service Profiler. What happened to it?

When you enable Application Insights Profiler, the Azure Service Profiler agent is disabled.

### <a id="double-counting"></a>Double counting in parallel threads

In some cases, the total time metric in the stack viewer is more than the duration of the request.

This might occur when there are two or more threads associated with a request, and they are operating in parallel. In that case, the total thread time is more than the elapsed time. One thread might be waiting on the other to be completed. The viewer tries to detect this and omits the uninteresting wait, but it errs on the side of showing too much rather than omitting what might be critical information.

When you see parallel threads in your traces, determine which threads are waiting so you can ascertain the critical path for the request. In most cases, the thread that quickly goes into a wait state is simply waiting on the other threads. Concentrate on the other threads and ignore the time in the waiting threads.

### <a id="issue-loading-trace-in-viewer"></a>No profiling data

Here are a few things that you can check:

* If the data you are trying to view is older than a couple of weeks, try limiting your time filter and try again.
* Check that proxies or a firewall have not blocked access to https://gateway.azureserviceprofiler.net.
* Check that the Application Insights instrumentation key you are using in your app is the same as the Application Insights resource that you used to enabled profiling. The key usually is in ApplicationInsights.config, but also might be located in the web.config or app.config files.

### Error report in the profiling viewer

Submit a support ticket in the portal. Be sure to include the correlation ID from the error message.

### Deployment error: Directory Not Empty 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

If you are redeploying your web app to an App Service resource with the profiler enabled, you might see a message that looks like the following:

Directory Not Empty 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

This error occurs if you run Web Deploy from scripts or from the Visual Studio Team Services Deployment Pipeline. The solution is to add the following additional deployment parameters to the Web Deploy task:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

These parameters delete the folder that's used by Application Insights Profiler and unblock the redeploy process. They don't affect the profiler instance that's currently running.


## Manual installation

When you configure the profiler, updates are made to the web app's settings. You can apply the updates manually if your environment requires it. For example, if your application runs in an App Service Environment for PowerApps.

1. In the web app control pane, open **Settings**.
2. Set **.Net Framework version** to **v4.6**.
3. Set **Always On** to **On**.
4. Add the __APPINSIGHTS_INSTRUMENTATIONKEY__ app setting and set the value to the same instrumentation key that's used by the SDK.
5. Open **Advanced Tools**.
6. Select **Go** to open the Kudu website.
7. On the Kudu website, select **Site extensions**.
8. Install __Application Insights__ from the Azure Web Apps Gallery.
9. Restart the web app.

## <a id="profileondemand"></a> Manually trigger profiler
When we developed the profiler, we added a command-line interface so that we could test the profiler on app services. Using these same interface users can also customize how the profiler starts. At a high level, the profiler uses App Service's Kudu System to manage profiling in the background. When you install the Application Insights Extension, we create a continuous web job which hosts the profiler. We use this same technology to create a new web job which you can customize to fit your needs.

This section explains how to:

1. Create a web job, which can start the profiler for two minutes with the press of a button.
2. Create a web job, which can schedule the profiler to run.
3. Set arguments for the profiler.


### Set up
First let's get familiar with the web job's dashboard. Under settings click on the WebJobs tab.

![webjobs blade](./media/app-insights-profiler/webjobs-blade.png)

As you can see this dashboard shows you all of the web jobs that are currently installed on your site. You can see the ApplicationInsightsProfiler2 web job which, has the profiler job running. This is where we end up creating our new web jobs for manual and scheduled profiling.

First let's get the binaries we need.

1.	Go to the Kudu site. Under the development tools tab, click on the "Advanced Tools" tab with the Kudu logo. Click on "Go". This takes you to a new site and logs you in automatically.
2.	Next we need to download the profiler binaries. Navigate to the File Explorer via Debug Console -> CMD located at the top of the page.
3.	Click on site -> wwwroot -> App_Data -> jobs -> continuous. You should see a folder "ApplicationInsightsProfiler2". Click on the download icon to the left of the folder. This downloads "ApplicationInsightsProfiler2.zip" file.
4.	This downloads all the files you need. I recommend creating a clean directory to move this zip archive to before moving on.

### Setting up the web job archive
When you add a new web job to the azure website basically, you create a zip archive with a run.cmd inside. The run.cmd tells the web job system what to do when you run the web job.

1.	To start create a new folder, our example is named "RunProfiler2Minutes".
2.	Copy the files from the extracted ApplicationInsightProfiler2 folder into this new folder.
3.	Create a new run.cmd file. (You can open this working folder in VS Code before starting for convenience.)
4.	Add the command `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`, and save the file.
a.	The `start` command tells the profiler to start.
b.	`--engine-mode immediate` tells the profiler we want to immediately start profiling.
c.	`--single` means to run and then stop automatically
d.	`--immediate-profiling-duration 120` means to have the profiler run for 120 seconds or 2 minutes.
5.	Save this file.
6.	Archive this folder, you can right-click the folder and choose Send to -> Compressed (zipped) folder. This creates a .zip file using the name of your folder.

![start profiler command](./media/app-insights-profiler/start-profiler-command.png)

We now have a web job .zip we can use to set up web jobs in our site.

### Add a new web job
Next we add a new web job in our site. This example shows you how to add a manually triggered web job. After you are able to do that the process is almost exactly the same for scheduled.

1.	Go to the web jobs dashboard.
2.	Click on the Add command from the toolbar.
3.	Give your web job a name. For clarity it can help to match the name of your archive and to open it up to have different versions of the run.cmd.
4.	In the file upload part of the form, click on the open file icon and find the .zip file you made above.
5.	For the type, choose Triggered.
6.	For the Triggers choose Manual.
7.	Hit OK to save.

![start profiler command](./media/app-insights-profiler/create-webjob.png)

### Run the profiler

Now that we have a new web job that we can trigger manually we can try to run it.

1. By design you can only have one ApplicationInsightsProfiler.exe process running on a machine at any given time. So to start off, make sure to disable the Continuous web job from this dashboard. Click on the row and press "Stop". Then select Refresh on the toolbar and confirm that the status indicates the job is stopped.
2. Click on the row with the new web job you've added and press run.
3. With the row still selected click on the Logs command in the toolbar, this brings you to a web jobs dashboard for the web job you have started. It lists the most recent runs and their results.
4. Click on the instance of run you've just started.
5. If all went well, you should see some diagnostic logs coming from the profiler confirming we have started profiling.

### Things to consider

Though this method is relatively straightforward, there are some things to consider.

- Since this is not managed by our service, we have no way of updating the agent binaries for your web job. We do not currently have a stable download page for our binaries so the only way to get the latest is by updating your extension and grabbing it from the continuous folder like we did in the previous steps.

- As this is utilizing command-line arguments that were originally designed for developer use rather than end-user use, these arguments may change in the future, so just be aware of that when upgrading. It shouldn't be much of a problem because you can add a web job, run, and test that it works. Eventually we will build a UI to handle this without the manual process.

- The Web Jobs feature for App Services is unique in that when it runs the web job it ensures that your process has the same environment variables and app settings that your web site will end up having. This means that you do not need to pass the instrumentation key through the command line to the profiler. It should just pick up the instrumentation key from the environment. However, if you want to run the profiler on your dev box or on a machine outside of App Services you need to supply an instrumentation key. You can do this by passing in an argument `--ikey <instrumentation-key>`. This value must match the instrumentation key your application is using. In the log output from the profiler, it tells you which ikey the profiler started with and if we detected activity from that instrumentation key while we are profiling.

- The manually triggered web jobs can actually be triggered via Web Hook. You can get this url by right-clicking on the web job from the dashboard and viewing the properties. Or by choosing properties in the toolbar after selecting the web job from the table. This opens up endless possibilities like triggering the profiler from your CI/CD pipeline (like VSTS) or something like Microsoft Flow (https://flow.microsoft.com/en-us/). Ultimately, this depends on how complex you want to make your run.cmd (which can also be a run.ps1), but the flexibility is there.

## Next steps

* [Working with Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
