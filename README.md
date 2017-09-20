# SerilogWeb.Owin

### Discontinued

This package has been discontinued due to the very minimal amount of code required to do this directly; to attach `RequestId` to Owin requests:

#### 1. Enable `LogContext`

In your Serilog configuration, add `Enrich.FromLogContext()`:

```csharp
Log.Logger = new LoggerConfiguration()
    .Enrich.FromLogContext()
    // Other sink configuration etc.
```

#### 2. Add the following Owin middleware

```csharp
app.Use(new Func<AppFunc, AppFunc>(next => (async env =>
{
    using (LogContext.PushProperty("RequestId", Guid.NewGuid()))
        await next.Invoke(env);
})));
```

(For other ways to add middleware to Owin apps, see [this post](http://benfoster.io/blog/how-to-write-owin-middleware-in-5-different-steps).
