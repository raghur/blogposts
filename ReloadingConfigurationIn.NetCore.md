<!--
PostId: 6044036057827443462
Title    : Reloading configuration in .NET core
Labels   : .netcore, tips
Format	 : markdown
Published: true
-->

Being able to reload configuration on change is typically a key requirement. ASP.NET core configuration infrastructure
provides a `reloadOnChange` property that you can set:

```csharp
var builder = new ConfigurationBuilder()
    .SetBasePath(env.ContentRootPath)
    .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
    .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
    .AddEnvironmentVariables();
Configuration = builder.Build();
```

However, your application still has to handle the change. In asp.net core, this is via a `IChangeToken` and the `Configuration`
class provides a `GetReloadToken().RegisterChangeCallback` - the only trouble is that this is fired only once since the
change token can change.

The easy way around this is to use the `ChangeToken` utility - like so:

```csharp

var valuesConfig = new ValuesConfig();
Configuration.GetSection("ValuesConfig").Bind(valuesConfig);
services.AddSingleton(valuesConfig);
Action onChange = () =>
{
    Configuration.GetSection("ValuesConfig").Bind(valuesConfig);
    Console.WriteLine("onChange fired!");
};

ChangeToken.OnChange(() => Configuration.GetReloadToken(), onChange);
```

What's interesting though that while this works, the on change handler ends up getting called twice for each  change!
