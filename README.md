# Orleans.TelemetryConsumers.Datadog

Orleans integration package for Datadog.

[![.NET](https://github.com/OrleansContrib/Orleans.TelemetryConsumers.Datadog/actions/workflows/dotnet.yml/badge.svg?branch=main)](https://github.com/OrleansContrib/Orleans.TelemetryConsumers.Datadog/actions/workflows/dotnet.yml)

## telemetry consumers

There are six telemetry consumer interfaces extending the `ITelemetryConsumer` interface:

* `IMetricTelemetryConsumer`
* `IDependencyTelemetryConsumer`
* `IEventTelemetryConsumer`
* `IExceptionTelemetryConsumer`
* `IRequestTelemetryConsumer`
* `ITraceTelemetryConsumer`

At the moment, only `IMetricTelemetryConsumer` is supported and it's implemented by `DatadogMetricTelemetryConsumer`.

### DatadogMetricTelemetryConsumer

Implements `IMetricTelemetryConsumer` interface. Provides basic metrics like grain count, information about application requests, messaging, etc.