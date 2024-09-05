# Example - Telemetry for .NET App

Basically, this example was created according to the instructions on the [Getting Started](https://opentelemetry.io/docs/languages/net/getting-started/) page.

The following adjustments have been made to ensure that the logs and traces are also sent to the collector:

```sh
> dotnet add package OpenTelemetry.Exporter.OpenTelemetryProtocol
```

**appsettings.json**

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    },
    "OpenTelemetry": {
      "IncludeFormattedMessage": true,
      "IncludeScopes": true,
      "ParseStateValues": true
    }
  },
  "Otlp": {
    "Endpoint": "http://localhost:4317"
  },
  "AllowedHosts": "*",
}
```

**Program.cs**

```csharp
builder.Services.AddOpenTelemetry()
    .ConfigureResource(resource => resource.AddService(serviceName))
    .WithTracing(tracing => tracing
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
        .AddOtlpExporter())
    .WithMetrics(metrics => metrics
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
        .AddOtlpExporter())
    .WithLogging(logs => logs
        .AddConsoleExporter()
        .AddOtlpExporter()
    );
```
