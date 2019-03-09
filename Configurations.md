# Customize Application Insights Profiler

## Ways of settings

There are several ways to customize the [profiler session life cycle](./ProfilerSessions.md) and other aspects of the profiler. Here's some common ways.

Generally, all Profiler sessions stays in the section of `ServiceProfiler`. The following settings will all short the session duration to 30 seconds, and it will also set the interval between sessions to 10 minutes.

### Using `appsettings.json`

```jsonc
{
  "ServiceProfiler": {
    "Duration": "00:00:30",
    "Interval": "00:10:00"
    // ...
  }
}
```

### Using environment variables

```bash
export ServiceProfiler__Duration="00:00:30"
export ServiceProfiler__Interval="00:10:00"
```

_Notice that `__` (2 underscores) are used to separate sections._

### Using the code

Overloads are provided to allow customize settings directly in code:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddServiceProfiler(options =>
    {
        options.Duration = TimeSpan.FromSeconds(30);
        options.Duration = TimeSpan.FromMinutes(10);
        // ... more options.
    });
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

All other ways document here: [Configuration in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2) are also supported.

## Configuration References

Here lists all supported configurations.

| Key                       | Value/Types | Default Value | Description                                                                                                                                                                   |
|---------------------------|-------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BufferSizeInMB            | Integer     | 250           | Circular Buffer size. For 2 minutes session, it usually generates less than 200MB traces. Reduce the size for saving resources, increase the size for longer sessions.        |
| Duration                  | TimeSpan    | 00:02:00      | The duration of a session.                                                                                                                                                    |
| Interval                  | TimeSpan    | 00:58:00      | The interval between sessions.                                                                                                                                                |
| InitialDelay              | TimeSpan    | 00:00:00      | Delay before starting the very first session after the application starts up.                                                                                                 |
| ProvideAnonymousTelemetry | Boolean     | true          | Sends anonymous telemetry data to Microsoft to make the product better when sets to true.                                                                                     |
| IsDisabled                | Boolean     | false         | Kill switch to turn off Profiler.                                                                                                                                             |
| IsSkipCompatibilityTest   | Boolean     | false         | Bypass the check of platform compatibility testing to turn Profiler on by force. This is introduced mainly for internal use for evaluation and might cause unexpected result. |
| SkipUpload                | Boolean     | false         | Skip uploading the traces to the back-end. Notice: Skipping uploading will always preserve the trace file.                                                                    |
| PreserveTraceFile         | Boolean     | false         | The trace file will be deleted once uploaded by default. Set this to true when you want to keep the local trace files.                                                        |

## Sets the logging level for Profiler

Sometimes, the Profiler might not run as expected. Set the log level to `Debug` or `Trace` will reveal a lot of related information. On the other hand, once running normally, set the log level to `Information` or above to reduce noise.

To do that, in `appsettings.json`:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning",
      "ServiceProfiler": "Debug"
    }
  }
}
```

Here's a complete example of `appsettings.json`:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning",
      "ServiceProfiler": "Debug"
    }
  },
  "AllowedHosts": "*",
  "ServiceProfiler": {
    "IsDisabled": false,
    "Duration": "00:00:30",
    "Interval": "00:10:00",
    "InitialDelay": "00:00:03",
    "ProvideAnonymousTelemetry": true,
    "IsSkipCompatibilityTest": false
  },
  "ApplicationInsights": {
    "InstrumentationKey": "application-insights-instrumentation-key"
  }
}
```