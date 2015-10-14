---
published: false
---

## Fixing Broken Azure Web Apps Diagnostics

Recently, Streaming Logs for an Azure Web App of mine broke. I was seeing the following when connecting to Streaming Logs:
```
Connecting to Application logs ...
2015-10-14T22:05:49  Welcome, you are now connected to log-streaming service.
Application: <!DOCTYPE html>
<html>
    <head>
        <title>Error reading JObject from JsonReader. Path '', line 0, position 0.</title>
        <meta name="viewport" content="width=device-width" />
        <style>
         body {font-family:"Verdana";font-weight:normal;font-size: .7em;color:black;} 
         p {font-family:"Verdana";font-weight:normal;color:black;margin-top: -5px}
         b {font-family:"Verdana";font-weight:bold;color:black;margin-top: -5px}
         H1 { font-family:"Verdana";font-weight:normal;font-size:18pt;color:red }
         H2 { font-family:"Verdana";font-weight:normal;font-size:14pt;color:maroon }
         pre {font-family:"Consolas","Lucida Console",Monospace;font-size:11pt;margin:0;padding:0.5em;line-height:14pt}
         .marker {font-weight: bold; color: black;text-decoration: none;}
         .version {color: gray;}
         .error {margin-bottom: 10px;}
         .expandable { text-decoration:underline; font-weight:bold; color:navy; cursor:hand; }
         @media screen and (max-width: 639px) {
          pre { width: 440px; overflow: auto; white-space: pre-wrap; word-wrap: break-word; }
         }
         @media screen and (max-width: 479px) {
          pre { width: 280px; }
         }
        </style>
    </head>

    <body bgcolor="white">

            <span><H1>Server Error in '/' Application.<hr width=100% size=1 color=silver></H1>

            <h2> <i>Error reading JObject from JsonReader. Path '', line 0, position 0.</i> </h2></span>

            <font face="Arial, Helvetica, Geneva, SunSans-Regular, sans-serif ">

            <b> Description: </b>An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code.

            <br><br>

            <b> Exception Details: </b>Newtonsoft.Json.JsonReaderException: Error reading JObject from JsonReader. Path '', line 0, position 0.<br><br>

            <b>Source Error:</b> <br><br>

            <table width=100% bgcolor="#ffffcc">
               <tr>
                  <td>
                      <code>

An unhandled exception was generated during the execution of the current web request. Information regarding the origin and location of the exception can be identified using the exception stack trace below.</code>

                  </td>
               </tr>
            </table>

            <br>

            <b>Stack Trace:</b> <br><br>

            <table width=100% bgcolor="#ffffcc">
               <tr>
                  <td>
                      <code><pre>

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
</pre></code>

                  </td>
               </tr>
            </table>

            <br>

            <hr width=100% size=1 color=silver>

            <b>Version Information:</b>&nbsp;Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.6.81.0

            </font>

    </body>
</html>
Application: <!-- 
Application: [JsonReaderException]: Error reading JObject from JsonReader. Path &#39;&#39;, line 0, position 0.
Application:    at Newtonsoft.Json.Linq.JObject.Load(JsonReader reader)
Application:    at Kudu.Core.Settings.JsonSettings.<Read>b__7()
Application:    at Kudu.Contracts.Infrastructure.LockExtensions.<>c__DisplayClass6`1.<LockOperation>b__5()
Application:    at Kudu.Contracts.Infrastructure.LockExtensions.TryLockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout)
Application:    at Kudu.Contracts.Infrastructure.LockExtensions.LockOperation[T](IOperationLock lockObj, Func`1 operation, TimeSpan timeout)
Application:    at Kudu.Core.Settings.JsonSettings.SetValue(String key, JToken value)
Application:    at Kudu.Services.Performance.LogStreamManager.<BeginProcessRequest>b__2()
Application:    at Kudu.Contracts.Infrastructure.LockExtensions.TryLockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout)
Application:    at Kudu.Contracts.Infrastructure.LockExtensions.LockOperation(IOperationLock lockObj, Action operation, TimeSpan timeout)
Application:    at Kudu.Services.Performance.LogStreamManager.BeginProcessRequest(HttpContext context, AsyncCallback cb, Object extraData)
Application:    at Kudu.Services.Performance.LogStreamHandler.BeginProcessRequest(HttpContext context, AsyncCallback cb, Object extraData)
Application:    at System.Web.HttpApplication.CallHandlerExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute()
Application:    at System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously)
Application: --><!-- 
Application: This error page might contain sensitive information because ASP.NET is configured to show verbose error messages using &lt;customErrors mode="Off"/&gt;. Consider using &lt;customErrors mode="On"/&gt; or &lt;customErrors mode="RemoteOnly"/&gt; in production environments.-->
Disconnected from Application logs
```

The solution was to delete `diagnostics\settings.json` which had somehow become an empty file.

In the Azure portal, navigate to your web site

```
del ..\diagnostics\settings.json
```
