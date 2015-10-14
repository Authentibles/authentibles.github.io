---
published: true
layout: post
date: 2015-08-15T12:00:00.000Z
categories: csharp fabric
---


Recently, Streaming Logs for an Azure Web App of mine broke. I was seeing the following when connecting to Streaming Logs:
```
[JsonReaderException: Error reading JObject from JsonReader. Path '', line 0, position 0.]
```

The solution was to delete `diagnostics\settings.json` which had somehow become an empty file.

In Visual Studio, open Server Explorer, expand `Files` in your Web App, expand `diagnostics`, and delete `settings.json`.

![Delete the file from Server Explorer](https://raw.githubusercontent.com/Authentibles/authentibles.github.io/master/images/easy-way.png)

Problem solved.

Alternatively, you can open the Azure Portal, navigate to your Web App, click `Tools`

![Click Tools](https://raw.githubusercontent.com/Authentibles/authentibles.github.io/master/images/click-tools.png)
 
Click `Console` and type the following:
```
del ..\diagnostics\settings.json
```
![Delete settings.json](https://raw.githubusercontent.com/Authentibles/authentibles.github.io/master/images/delete-settings.png)
Hope this helps someone!

Here's the stack trace of the error:

```
[JsonReaderException: Error reading JObject from JsonReader. Path &#39;&#39;, line 0, position 0.]
   Newtonsoft.Json.Linq.JObject.Load(JsonReader reader) +406
   Kudu.Core.Settings.JsonSettings.&lt;Read&gt;b__7() +188
   Kudu.Contracts.Infrastructure.&lt;&gt;c__DisplayClass6`1.&lt;LockOperation&gt;b__5() +19
   Kudu.Contracts.Infrastructure.LockExtensions.TryLockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout) +185
   Kudu.Contracts.Infrastructure.LockExtensions.LockOperation(IOperationLock lockObj, Func`1 operation, TimeSpan timeout) +138
   Kudu.Core.Settings.JsonSettings.SetValue(String key, JToken value) +168
   Kudu.Services.Performance.LogStreamManager.&lt;BeginProcessRequest&gt;b__2() +163
   Kudu.Contracts.Infrastructure.LockExtensions.TryLockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout) +185
   Kudu.Contracts.Infrastructure.LockExtensions.LockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout) +22
   Kudu.Services.Performance.LogStreamManager.BeginProcessRequest(HttpContext context, AsyncCallback cb, Object extraData) +459
   Kudu.Services.Performance.LogStreamHandler.BeginProcessRequest(HttpContext context, AsyncCallback cb, Object extraData) +203
   System.Web.CallHandlerExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +1055
   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean&amp; completedSynchronously) +137
```
