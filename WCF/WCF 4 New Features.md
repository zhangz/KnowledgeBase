## Simplified configuration
This new feature shows simplification of WCF configuration section by providing default endpoint, binding and behavior configuration. It is not mandatory to provide endpoint while hosting service. Service will automatically create new endpoint if it does find any endpoint while hosting service. These changes make it possible to host configuration-free services.

### Default endpoints
- One default endpoint per contract/base address combination
- If you have defined at least one endpoint, you won't get default endpoints

### Default bindings & behaviors
- Simply define the binding/behavior without a name
- That binding takes effect for anything not assigned a specific binding
- Can be defined at different scope within config (e.g. machine.config)

### Common behavior configuration
- Only allowed in machine.config
- Additive to other configured behaviors

### Standard endpoints
WCF 4.0 comes with several preconfigured standard endpoints for Discovery, MetadataExchange, REST and WF integration.

- Those cover some of the most common usage scenarios
- You can simply reuse them as-is without changes
- You refer to standard endpoints by name using the "kind" attribute

### File-less activation
In WCF 4.0, you no longer need physical .svc files to activate sercices. Instead you can define a virtual mapping in config. Hosting service can be done without .svc file.

## REST Service Updates
- Automatic help page that describes REST services to consumer
- Support for declarative HTTP catching
- Message format selection
- WebFaultException
- Integration with ASP.NET routes
- New project templates

## Discovery Service
Service discovery makes it possible to discover endpoints dynamically. Allows for dynamic configuration of services. This can be achieved using WS-Discovery protocol.

- Ad-hoc discovery
- Managed mode discovery 

## Routing Service
This new feature introduces routing service between client and actual business service. This intermediated service act as broker or gateways to the actual business services and provides features for content based routing, protocol bridging and error handling

Use cases:
- Bridge protocols (HTTP -> TCP)
- Well known public face for services. Abstract deployment of other services.
- Loading balancing of service calls
- Hanlde failover
- Multicast one-way messages to well known receivers