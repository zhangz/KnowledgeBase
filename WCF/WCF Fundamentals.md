# Programming Services

### Hosting WCF services
- Self hosting: `ServiceHost` class
- Managed hosting

### Configure a service within a host
WCF let you configure services in either code or configuration. Use `SvcConfigEdtior.exe` to work with the WCF config section (right click app.config file, and select Edit WCF Configuration).

- Endpoints: address, binding, contract
- Behaviors
- Binding configurations

### WCF address
- Endpoints can share an address if they share the same binding instance.
- But they ust be configured with different contracts.

### WCF bindings
A binding specifies three key communication details:

- Transport protocols: HTTP,TCP,MSMQ,etc
- Message format: XML,JSON,binary,etc
- Message protocols: SOAP, WS-*,none,etc

WCF built-in bindings:

- WebHttpBinding: REST style communication
- BasicHttpBinding: basic SOAP communication
- NetTcpBinding: SOAP + WS-* over TCP
- NetMsmqBinding: SOAP + WS-* over MSMQ
- ...

### Apply service behaviors
There are several ways to apply behaviors to your services (code, or configuration)

- Attributes
- ServiceHost
- Configuration

### Publish service metadata
- WCF can automatically publish WSDL
	- Enable via `ServiceMetaBehavior`
- Easy to expose WS-MetadataExchange (MEX)
	- A standard service contract for discovering other endpoints 
	- Enable by adding `IMetadataExchange` endpoints
	- Choose one of the built-in MEX bindings for HTTP,TCP,pipes

### Service exceptions
Instruct WCF to include the exception details in faults

- Enable with `[ServiceBehavior]` or the `<serviceDebug>` behavior (includeExceptionDetailInFaults='true')
- Useful for debugging purposes, not recommended in production

`FaultException` class represents an explicit SOAP fault

# Programming Clients

In order for client application to consume WCF service, first it needs to retrieve the service's metadata. The service will expose the metadata typically in the form of a WSDL definition. The client will retrieve that WSDL definition and run it through a client-side metadata import tool. By doing that in WCF that will produce client-side endpoint definitions that represents the information needed to communicate with that service through one of its endpoints. Client, once it has those endpoint definitions, can then choose an endpoint and construct a channel based on that endpoint. Then it can begin sending messages to the service through that channel by simply making method calls on the channel instance. 

### Retrieve endpoint definitions
Client can automatically generate endpoint definitions from WSDL

- Generate equivalent contract definitions (code)
- Generate equivalent endpoint definitions (configuration)

Two ways:

1. `SvcUtil.exe` is a metadata import tool for producing WCF client code
	- Downloads WSDL and generates WCF code and configuration
	- Supports MEX retrieval over HTTP,TCP,pipes
2. `Add Service Reference` feature in VS
	- So you don't have to run SvcUtil.exe directly in most cases
	- Provides most of same functionality
	- Generates code/configuration and adds it to the project 

### Programming WCF channels
1. Create and configure a ChannelFactory
2. Create a channel instance
3. Make method calls through channel
4. Close or abort the channel

Close vs. Abort:

- Close performs a graceful shutdown of the client channel
	- Waits for in-progress calls to complete before closing
	- Close underlying networking resources
	- Can throw CommunicationException & TimeoutException
- Abort tears down the client channel immediately
	- Aborts in-progress calls and closes channel immediately
- You must call Abort on 'faulted' channels
	- Typical after certain types of exceptions 

All channels implement `IClientChannel` for lifecyle management.

Avoiding `ChannelFactory<T>`:

- The import tools generate a proxy class for each service contract type. Proxy class name = service contract name (minus the "I") + "Client".

### Configure client channels:

- Client channels can be configured via the binding & behaviors
- Key things to configure on the binding:
	- `sendTimeout` value - influence when the TimeoutException occurs
	- Transport/protocol specific settings

### Handling exceptions
Client channels need to be prepared for two main exception types:

- `CommunicationException` 
	- Various runtime communication errors
	- `FaultException`'s thrown by operations
- `TimeoutException`
	- `sendTimeout` limit exceeded
	- Thrown by underlying transport channel 

### Invoking services asynchronously
- Use the `/async /targetClientVersion:Version35` switch when generating the client code (check 'Generate asynchronous operation' in 'Configure Service Reference' menu item)
- This produces an XXXAsync method + an event for each operation
- Simply hook up event and call the asynchronous method

### Sharing service contract assemblies
Tell import tools to reuse types in referenced assemblies.

- Use `/reference` (SvcUtil.exe) or the Service Reference Settings (or just check `Reuse types in referenced assemblies`). Then the tool only generates config, not contracts.

### Programming MEX
WCF provides offical support for MEX in the programming model.
- Use `MeatdataResolver` and/or `MetadataExchangeClient`
- Allows you to programmatically retrieve metadata at runtime
- Clients can dynamically choose endpoints to use

# Restful Services
REST is an acronym standing for Representational State Transfer.

SOAP vs. REST:

- REST is not a protocol like SOAP is; it is an architectural design for building services that build on HTTP.
- SOAP defines a transport-neutral model focused on custom operations
- REST defines a transport-specific (HTTP) model focused on resources
- SOAP emphasizes *verbs* while SOAP emphasizes *nouns* or '*resources*'
	- With SOAP, you define *custom operations* tunnels though POST
	- With REST, you define *resources*, and use a *uniform interface* to operate on them (HTTP verbs)
 
With REST, you build resource-oriented architecture (ROA)

- You focus on identifying & naming resources (URIs)
- How to represent them (XML, JSON, RSS format)
- How to navigate between them (HREFs)
- You use a uniform interface to interact with them (HTTP verbs)

Primary Benefits

- Widespread interoperability, basically anyone who supports HTTP and XML messages can use your service.
- Scalability, because we are leveraging HTTP, we can now take advantage of the entire wide spread web infrastructure and optimization built around that specific protocol and those specific operations.

The web is primarily built on GET

- GET simply means "retrieve" the representation of a resource
- Should not cause any insafe side-effects
- Idempotency expected

### Web Programming Model Comparison
REST

- An architecture model that describes how to identify, represent and operate on resources
- Embraces HTTP verbs, URIs, and web data formats
- Action defined by HTTP verbs

SOAP

- A model for defining higher level application protocols
- Can be used with any transport (HTTP,TCP,MSMQ,etc)
- Only POST when used with HTTP, action conveyed in SOAP 

POX

- It is just exchanging XML message over HTTP without SOAP, referred to as plain -Old XML (POX).

### REST Support in WCF
- WebHttpBinding
- WebHttpBehavior
- WebServiceHost
	- Automatically adds endpoint using WebHttpBinding
	- Automatically adds WebHttpBehavior to the endpoint
- WebGetAttribute
	- maps HTTP GET requests to a WCF method
- WebInvokeAttribute
	- maps all other HTTP verbs to WCF methods
- UriTemplate
- WebOperationContext
- WebMessageFormat (XML, JSON)
- WebScriptServiceHostFactory - enable AJAX integration with the approprite binding, behavior added automatically



