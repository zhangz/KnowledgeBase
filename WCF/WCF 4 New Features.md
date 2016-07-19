## Configuration Model Updates

### Default endpoints
- One default endpoint per contract/base address combination
- If you have defined at least one endpoint, you won't get default endpoints

### Default bindings & behaviors
- Simply define the binding/behavior without a name
- That binding takes effect for anything not assigned a specific binding
- Can be defined at different scope within config (e.g. machine.config)

[http://wcftutorial.net/binding-and-behavior.aspx](http://wcftutorial.net/binding-and-behavior.aspx)

### Common behavior configuration
- Only allowed in machine.config
- Additive to other configured behaviors

### Standard endpoints
WCF 4.0 comes with several preconfigured standard endpoints for Discovery, MetadataExchange, REST and WF integration.

- Those cover some of the most common usage scenarios
- You can simply reuse them as-is without changes
- You refer to standard endpoints by name using the "kind" attribute

### File-less activation
In WCF 4.0, you no longer need physical .svc files to activate sercices. Instead you can define a virtual mapping in config. Can now host without .svc file.

## REST Updates
- Automatic help
- HTTP caching support
- Message format selection
- WebFaultException
- Integration with ASP.NET routes
- New project templates

## Discovery Services
Service discovery makes it possible to discover endpoints dynamically. Allows for dynamic configuration of services.

- Ad-hoc discovery
- Managed mode discovery 

## Routing

Use cases:

- Bridge protocols (HTTP -> TCP)
- Well known public face for services. Abstract deployment of other services.
- Loading balancing of service calls
- Hanlde failover
- Multicast one-way messages to well known receivers