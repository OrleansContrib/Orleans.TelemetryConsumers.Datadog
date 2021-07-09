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

## usage

`DatadogMetricTelemetryConsumer` has two constructors:

* `DatadogMetricTelemetryConsumer(string[] namesOfRelevantMetrics, StatsdConfig statsdConfig)` - accepts a collection of metric names to be tracked and datadog statsd configuration

  ```c#
    siloBuilder.ConfigureServices(services =>
    {
      services.AddSingleton(serviceProvider =>
          new DatadogMetricTelemetryConsumer(
              new[] {"App.Requests.Total.Requests.Current"},
              new StatsdConfig
              {
                  StatsdServerName = "127.0.0.1",
                  StatsdPort = 8125
              }));
      services.Configure<TelemetryOptions>(
          telemetryOptions => telemetryOptions.AddConsumer<DatadogMetricTelemetryConsumer>());
    });
  ```

  Here, Orleans will detect that `DatadogMetricTelemetryConsumer` is already registered in the collection of silo's services (the `AddSingleton` bit) and will not instantiate it again.

  More on statsd configuration here: https://docs.datadoghq.com/developers/service_checks/dogstatsd_service_checks_submission/

* `DatadogMetricTelemetryConsumer()` - Orleans uses this one by default if `DatadogMetricTelemetryConsumer` instance cannot be found in the collection of silo's services.

  ```c#
  siloBuilder.ConfigureServices(services =>
  {
      services.Configure<TelemetryOptions>(
          telemetryOptions => telemetryOptions.AddConsumer<DatadogMetricTelemetryConsumer>());
  });
  ```

  Here, Orleans will instantiate `DatadogMetricTelemetryConsumer` using the default constructor.

A fleshed out example can be found in e.g. [the road to orleans](https://github.com/PiotrJustyna/road-to-orleans/tree/main/3b).