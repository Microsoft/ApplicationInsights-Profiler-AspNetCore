# Application Insights Profiler for Asp.Net core on Linux App Services

This is the project home page for App Services Linux profiler. Our NuGet package can be found [here](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Profiler.AspNetCore/).

**Notice:** It is highly recommended to use `Application Insights Profiler` with [CLR 2.1](https://dot.net) (shipped with Dotnet Core SDK 2.1.300) and above since there are fixes for fundamental reliability issues.

![Profiler Traces](https://raw.githubusercontent.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/master/media/profiler-traces.png)

## Get Started

* Create a WebApi project
    ```csharp
    dotnet new webapi -n ProfilerEnabledWebAPI
    ```
* Add the NuGet package
    ```csharp
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore -v 1.1.4-*
    ```
    _Note: Find the latest package from the [NuGet.org here](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Profiler.AspNetCore/)._

* [Create an Application Insights in Azure Portal](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-dotnetcore-quick-start?toc=/azure/azure-monitor/toc.json#log-in-to-the-azure-portal), set Application Insights instrumentation key in `appsettings.json`:
    ```json
    {
        "ApplicationInsights": {
            "InstrumentationKey": "replace-with-your-instrumentation-key"
        }
    }
    ```

* Enable Application Insights in `Program.cs`:
    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseApplicationInsights() // Add this line of code to Enable Application Insights
            .UseStartup<Startup>();
    ```

* Enable Profiler in `Startup.cs`:
    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddServiceProfiler(); // Add this line of code to Enable Profiler
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

* Run the code:
    ```bash
    dotnet run
    ```
    And you will see the following logs, indicating Profiler is up and running:
    ```bash
    dbug: ServiceProfiler.EventPipe.AspNetCore.ServiceProfilerStartupFilter[0]
        Constructing ServiceProfilerStartupFilter
    dbug: ServiceProfiler.EventPipe.AspNetCore.ServiceProfilerStartupFilter[0]
        User Settings:
        {
            "BufferSizeInMB": 250,
        }
    dbug: ServiceProfiler.EventPipe.Client.Utilities.RuntimeCompatibilityUtility[0]
        Checking compatibility for Linux platform.
    dbug: ServiceProfiler.EventPipe.AspNetCore.ServiceProfilerStartupFilter[0]
        Pass Runtime Compatibility test.
    dbug: ServiceProfiler.EventPipe.Client.Schedules.TraceSchedule[0]
        Entering TriggerStart - initial: True.
    dbug: ServiceProfiler.EventPipe.Client.Schedules.TraceSchedule[0]
        Entering StartAsync, idle for the interval of 3000 ms.
    Hosting environment: Development
    Content root path: /mnt/d/ApplicationInsightsProfiler
    Now listening on: https://localhost:5001
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ...
    ```
You have been start to run the the WebApi with Profiler on.

## Learn More

* [Profiler Sessions](./ProfilerSessions.md) - describes when the profiler starts, stops and what is traced.
* [Configurations for the Profiler](./Configurations.md) - describes how to customize various settings of the profiler.
* [Trace Analysis](./https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json#view-profiler-data) - introduce the trace analysis.

## Supported Versions

| Application Insights Profiler | Windows                                                                                        | Linux                            |
|-------------------------------|------------------------------------------------------------------------------------------------|----------------------------------|
| 1.1.4-beta1                   | Experimental support for .NET Core App 2.2. Trace tree in the trace explorer looks very noisy. | Supported for .NET Core App 2.1+ |
| 1.1.3-beta2                   | Not supported.                                                                                 | Supported for .NET Core App 2.1+ |
| 1.1.3-beta1                   | Not supported.                                                                                 | Supported for .NET Core App 2.1+ |
| 1.1.2-beta1                   | Not supported.                                                                                 | Deprecated.                      |
| 1.0.0-beta1                   | Not supported.                                                                                 | Deprecated.                      |


## Examples

* [Enable Service Profiler for containerized ASP.NET Core application](./examples/EnableServiceProfilerCLR2_1/README.md).

* [Enable Service Profiler for ASP.NET Core application in Visual Studio](./examples/EnableServiceProfilerInVSCLR2_1).

## References

* [Profile ASP.NET Core Azure Linux web apps with Application Insights Profiler](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler-aspnetcore-linux)

## CAUTION

This is a documentation/sample repository. The [LICENSE](LICENSE) covers the content in this repository but does **NOT** cover the use of the product of Microsoft.ApplicationInsights.Profiler.AspNetCore. Please reference [EULA-prerelease.md](EULA-prerelease.md) for any prerelase product and [EULA-GA.md](EULA-GA.md) for any non-prerelease product.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
