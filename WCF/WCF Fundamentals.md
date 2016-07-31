# Programming Services
http://wcftutorial.net

## Hosting WCF services
- Self hosting: WCF service can be self hosted as console application, WinForms or WPF application.
- Windows Service: hosted as a Windows Service, so that it is under control of the Service Control Manager (SCM).
- IIS: The main advantage of hosting service in IIS is that, it will automatically launch the host process when it gets the first client request. It uses the features of IIS such as process recycling, idle shutdown, process health monitoring and message based activation. The main disadvantage of using IIS is that, it will support only HTTP protocol.
- Windows Activation Service (WAS): it is the new process activation mechanism that ships with IIS 7.0. It supports Http, TCP and named pipes while IIS 6.0 supports only Http.

`ServiceHost` (`WebServiceHost`) object is in the process of hosting the WCF service and registering endpoints. It loads the service configuration endpoints, apply the settings and start the listeners to handle the incoming request. This object is created while self hosting the WCF service.

```csharp
//Creating uri for the hosting the service
Uri uri = new Uri("http://localhost/MathService");
//Creating the host object for MathService
ServiceHost host = new ServiceHost(typeof(MathService), uri);
//Adding endpoint to the Host object
host.AddServiceEndpoint(typeof(IMathService), new WSHttpBinding(), uri);
host.Open(); //Hosting the Service
```

## Configure a service within a host
WCF let you configure services in either code or configuration. Use `SvcConfigEdtior.exe` to work with the WCF config section (right click app.config file, and select Edit WCF Configuration).

- Endpoints: address, binding, contract
- Behaviors
- Binding configurations

## Endpoint
WCF Service exposes a collection of endpoints. Each endpoint is a portal for communicating with the world. All the WCF communications are taken place through endpoint. End point consists of three components.

### Address - where?
Basically URI, specifies where this WCF service is hosted. Client will use this uri to connect to the service.
- Endpoints can share an address if they share the same binding instance.
- But they ust be configured with different contracts.

### Binding - how?
Binding will describes how client will communicate with service. There are different protocols available for the WCF to communicate to the client.

Binding can be configured either through configuration file or code.

A binding specifies three key communication details:

- Transport: HTTP,TCP,MSMQ,etc
- Encoding (Optional): Message format - XML,JSON,binary,etc
- Protocol (Optional): SOAP, WS-*,none,etc

WCF built-in bindings:

- WebHttpBinding: REST style communication
- BasicHttpBinding: basic SOAP communication
- NetTcpBinding: SOAP + WS-* over TCP, across computers, supports duplex contracts and transactions
- NetNamedPipeBinding: Communication between WCF applications on same computer. Supports duplex contracts and transactions
- NetMsmqBinding: SOAP + WS-* over MSMQ
- mexHttpBinding: The Metadata Exchange endpoint is used by the service to describe itself to clients
- ...

##### Bindings and Channel Stacks

In WCF all the communication details are handled by channel, it is a stack of channel components that all messages pass through during runtime processing. The transport channel implements the given transport protocol and reads incoming messages off the wire. The transport channel uses a message encoder to read the incoming bytes into a logical Message object for further processing.

After that, the message bubbles up through the rest of the channel stack, giving each protocol channel an opportunity to do its processing, until it eventually reaches the top and WCF dispatches the final message to your service implementation. Messages undergo significant transformation along the way.

WCF provides easy way of achieving this using endpoint. In endpoint we will specify address, binding and contract. WCF follows the instructions outlined by the binding description to create each channel stack. The binding binds your service implementation to the wire through the channel stack in the middle.

### Contract - what?
In WCF, all services are exposed as contracts. Contract is a platform-neutral and standard way of describing what the service does. Each operation is a simple exchange pattern such as one-way, duplex and request/reply. Mainly there are four types of contracts available in WCF.

#### ServiceContract
It describes the operation that service can provide. Service Contract can be define using `[ServiceContract]` and `[OperationContract]` attribute. `[ServiceContract]` attribute is similar to the `[WebServcie]` attribute in the WebService and `[OpeartionContract]` is similar to the `[WebMethod]` in WebService.

- It describes the client-callable operations (functions) exposed by the service
- It maps the interface and methods of your service to a platform-independent description
- It describes message exchange patterns that the service can have with another party. Some service operations might be one-way; others might require a request-reply pattern

#### DataContract
It describes the custom data type which is exposed to the client. This defines the data types, that are passed to and from service.

Data contract can be explicit or implicit. Simple type such as int, string etc has an implicit data contract. User defined object are explicit or Complex type, for which you have to define a Data contract using [DataContract] and [DataMember] attribute.

A data contract can be defined as follows:
- It describes the external format of data passed to and from service operations
- It defines the structure and types of data exchanged in service messages
- It maps a CLR type to an XML Schema
- It defines how data types are serialized and deserialized. Through serialization, you convert an object into a sequence of bytes that can be transmitted over a network. Through deserialization, you reassemble an object from a sequence of bytes that you receive from a calling application.
- It is a versioning system that allows you to manage changes to structured data

#### MessageContract
Default SOAP message format is provided by the WCF runtime for communication between Client and service. If it is not meeting your requirements then we can create our own message format. This can be achieved by using Message Contract attribute.

SOAP message contain Envelope, Header and Body. SOAP Envelope contails name, namespace, header and body element. SOAP Header contain important information which are not directly related to message. SOAP body contains information which is used by the target.

There are three way of communication between source and destination:
- Simplex - It is one way communication. Source will send message to target, but target will not respond to the message.
- Request/Replay - It is two way communications, when source send message to the target, it will resend response message to the source. But at a time only one can send a message
- Duplex - It is two way communication, both source and target can send and receive message simultaniouly.

WCF supports either RPC(Remote Procedure Call) or Message style operation model. In the RPC model, you can develop operation with Ref and out parameter. WCF will automatically create the message for operation at run time. In Message style operation WCF allows to customize the message header and define the security for header and body of the message.

```csharp
[MessageContract]
public class EmployeeDetails
{
    [MessageHeader]
    public string EmpID;
    [MessageBodyMember]
    public string Name;
    [MessageBodyMember]
    public string Designation;
    [MessageBodyMember]
    public int Salary;
    [MessageBodyMember]
    public string Location;
}
```
When I use this EmployeeDeatils type in the service operation as parameter. WCF will add extra header call 'EmpID' to the SOAP envelope. It also add Name, Designation, Salary, Location as extra member to the SOAP Body.

- `MessageHeaderArray` Attribute
- Message Contract Properties
	- `ProtectionLevel`
	- `Name` and `Namespace`
	- `Order`

Rules :

You have to follow certain rules while working with Message contract

- When using Message contract type as parameter, Only one parameter can be used in servicie Operation
```csharp
[OperationContract]
void SaveEmployeeDetails(EmployeeDetails emp);
```
- Service operation either should return Messagecontract type or it should not return any value
```csharp
[OperationContract]
EmployeeDetails GetEmployeeDetails();
```
- Service operation will accept and return only message contract type. Other data types are not allowed.
```csharp
[OperationContract]
EmployeeDetails ModifyEmployeeDetails(EmployeeDetails emp);
```

Note: If a type has both Message and Data contract, service operation will accept only message contract.

#### FaultContract
By default when we throw any exception from service, it will not reach the client side. WCF provides the option to handle and convey the error message to client from service using SOAP Fault contract. It provides documented view for error occurred in the service to client. This helps us to easy identity, what error has occurred.

If you want to send exception information form service to client, you have to use FaultException as shown below.
```csharp
public int Add(int num1, int num2)
{
	//Do something
	throw new FaultException("Error while adding number");

}
```

You can also create your own Custom type and send the error information to the client using `FaultContract`. These are the steps to be followed to create the fault contract.
- Define a type using the data contract and specify the fields you want to return.
- Decorate the service operation with the FaultContract attribute and specify the type name.
- Raise the exception from the service by creating an instance and assigning properties of the custom exception.

```csharp
// Defining the type using Data Contract
[DataContract]
public class CustomException
{
	[DataMember]
	public string Title;
	[DataMember]
	public string ExceptionMessage;
	[DataMember]
	public string InnerException;
	[DataMember]
	public string StackTrace;
}

// Decorate the service operation with the FaultContract
[ServiceContract]
public interface ISimpleCalculator
{
	[OperationContract]
	[FaultContract(typeof(CustomException))]
	int Add(int num1, int num2);
}

// Raise the exception from the service
public int Add(int num1, int num2)
{
	//Do something
	CustomException ex = new CustomException();
	ex.Title = "Error Funtion:Add()";
	ex.ExceptionMessage = "Error occur while doing add function.";
	ex.InnerException = "Inner exception message from serice";
	ex.StackTrace = "Stack Trace message from service.";
	throw new FaultException(ex, "Reason: Testing the Fault contract");
}

// On client side, you can capture the service exception and process the information
try
{
	MyCalculatorServiceProxy.MyCalculatorServiceProxy proxy
	= new MyCalculatorServiceProxy.MyCalculatorServiceProxy();
	Console.WriteLine("Client is running at " + DateTime.Now.ToString());
	Console.WriteLine("Sum of two numbers... 5+5 =" + proxy.Add(5, 5));
	Console.ReadLine();
}
catch (FaultException<MyCalculatorService.CustomException> ex)
{
	//Process the Exception
}
```

#### Example
```csharp
[DataContract]
public class Eval
{
    [DataMember]
    public string Submitter;
    [DataMember]
    public DateTime Timesent;
    [DataMember]
    public string Comments;
}

[ServiceContract]
public interface IEvalService
{
    [OperationContract]
    void SubmitEval(Eval eval);
    [OperationContract]
    List<Eval> GetEvals();
}
```

Configuration:
```xml
<system.serviceModel>
	<services>
		<service name="MathService"
			behaviorConfiguration="MathServiceBehavior">
			<endpoint
				address="http://localhost:8090/MyService/MathService.svc" contract="IMathService"
				 binding="wsHttpBinding" bindingName="wshttpbind"/>
		</service>
	</services>
 	<bindings>
    	<wsHttpBinding>
        	<binding name="wshttpbind"  allowCookies="true" closeTimeout="00:01:00" 
        receiveTimeout="00:01:00" />
    	</wsHttpBinding>
  	</bindings>
	<behaviors>
		<serviceBehaviors>
			<behavior name="MathServiceBehavior">
				< !--To avoid disclosing metadata information,
				set the value below to false and remove the metadata endpoint above before deployment -->
				<serviceMetadata httpGetEnabled = "True" />

				< !--To receive exception details in faults for debugging purposes,
				set the value below to true.  Set to false before deployment 
				to avoid disclosing exception information -->
				<serviceDebug includeExceptionDetailInFaults = "False" />
			</behavior>
		</serviceBehaviors>
	</behaviors>
</system.serviceModel>
```

## Apply service behaviors
There are several ways to apply behaviors to your services (code, or configuration)

- Attributes
- ServiceHost
- Configuration

Behaviors scope:
- Common behaviors affect all endpoints globally - only allowed in machine.config
- Service behaviors affect only service-related aspects - app.config or `serviceHost.Description.Behaviors`
- Endpoint behaviors affect only endpoint-related properties - `serviceHost.Description.Endpoints[0].Behaviors`
- Operation-level behaviors affect particular operations.

WCF behaviors:
- Throttling Behavior - Controls how many messages are processed.
- Error Behavior - Specifies what occurs, when internal error occurs on the service.
- Metadata Behavior - Tells how and whether metadata is available to outside world.
- Instance Behavior - Specifies how many instance of the service has to be created while running.
- Transaction Behavior - Enables the rollback of transacted operations if a failure occurs.
- Dispatch Behavior - Controls how a message is processed by the WCF Infrastructure.

Behavior extensions:
It enables the user to create user-defined behavior elements. These elements can be used alongside the standard WCF behavior elements.

```xml
<extensions>
  <behaviorExtensions>
     <add name="protobuf" type="ProtoBuf.ServiceModel.ProtoBehaviorExtension, protobuf-net, Version=2.0.0.668, Culture=neutral, PublicKeyToken=257b51d87d2e4d67"/>
  </behaviorExtensions>
</extensions>
```


```csharp
[ServiceBehavior(InstanceContextMode=InstanceContextMode.Single)]
public class EvalService : IEvalService
{
    List<Eval> evals = new List<Eval>();

    #region IEvalService Members

    public void SubmitEval(Eval eval)
    {
        evals.Add(eval);
    }

    public List<Eval> GetEvals()
    {
        return evals;
    }

    #endregion
}
```

## Throttling
WCF throttling provides some properties that you can use to limit how many instances or sessions are created at the application level.

Service Throttling can be configured via either configuration file or code
#### Configuration file
Using `<serviceThrottling>` tag of the `serviceBehaviors`, you can configure the `maxConcurrentCalls`, `maxConcurrentInstances`, `maxConcurrentSessions` property as shown below.

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name="ServiceBehavior">
      <serviceMetadata httpGetEnabled="true"/>
      <serviceDebug includeExceptionDetailInFaults="true "/>
      <serviceThrottling maxConcurrentCalls="500"
 		maxConcurrentInstances ="100"
		maxConcurrentSessions ="200"/>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

#### Code 
Use `ServiceThrottlingBehavior` object to set concurrent calls, session and instance property.
```csharp
ServiceHost host = new ServiceHost(typeof(MyService));
ServiceThrottlingBehavior throttle = host.Description.Behaviors.Find();
if (throttle == null)
{
    throttle = new ServiceThrottlingBehavior();
throttle.MaxConcurrentCalls = 500;
    throttle.MaxConcurrentSessions = 200;
    throttle.MaxConcurrentInstances = 100;
    host.Description.Behaviors.Add(throttle);
}
host.Open();
```

## Instance Management
Instance management refers to the way a service handles a request from a client. Instance management is set of techniques WCF uses to bind client request to service instance, governing which service instance handles which client request. It is necessary because application will differ in their need for scalability, performance, durability, transaction and queued calls.

Basically there are three instance modes in WCF:
- PerCall: service instance will be created for each client request. The instance will be disposed after response is sent back to client.
- PerSession: logical session between client and service will be maintained. When the client creates new proxy to particular service instance, a dedicated service instance will be provided to the client. It is independent of all other instance.
- Single: all clients are independently connected to the same single instance. This singleton instance will be created when service is hosted and, it is disposed when host shuts down.

In most cases use the `InstanceContextMode` to `PerCall` as default.

## Instance Deactivation
Basically service instance is hosted in a context. Session actually correlated the client message not to the instance, but to the context that host it. When session starts, context is created and when it closes, context is terminated. WCF provides the option of separating the two lifetimes and deactivating the instance separately from its context.

`ReleaseInstanceMode` property of the `OberationalBehavior` attribute used to control the instance in relation to the method call.

Followings are the list Release mode available in the `ReleaseInstanceMode`
- RealeaseInstanceMode.None
- RealeaseInstanceMode.BeforeCall
- RealeaseInstanceMode.AfterCall
- RealeaseInstanceMode.BeforeAndAfterCall

You can also explicitly deactivate instance using InstanceContext object as shown below.
```csharp
OperationContext.Current.InstanceContext.ReleaseServiceInstance();
```

## Message and Channel
WCF Message is the unit of data exchange between client and service. It consists of several parts, including a body and headers.

Messaging layer is composed of channels. Channels are the core abstraction for sending message to and receiving message from an Endpoint. Broadly we can categories channels as:

Transport Channels
- Handles sending and receiving message from network. Protocols like HTTP, TCP name pipes and MSMQ.

Protocol Channels
- Implements SOAP based protocol by processing and possibly modifying message. e.g. WS-Security and WS-Reliability.

## Publish service metadata
Characteristics of the service are described by the metadata. This metadata can be exposed to the client to understand the communication with service.

WCF provides rich infrastructure for Exporting, Publishing, retrieving and Importing the metadata. Now we are going to focus mainly on publishing metadata. By default service metadata is turn-off due to security reason.

There are two way to publish metadata, either we can use HTTP-GET or through message exchange endpoint:

#### HTTP GET
WCF can automatically publish WSDL using HTTP GET, this can be done either in configuration file or code:
- Enable the `ServiceMetadata` node inside the `serviceBehaviors` node of the service configuration file
	```xml	
	<serviceMetadata httpGetEnabled="True"/>
	```

- Use `ServiceMetadataBehavior` in code
	```csharp
	//Enable metadata exchange - you need this so others can create proxies
	//to consume your WCF service
	ServiceMetadataBehavior serviceMetaBehavior = new ServiceMetadataBehavior();
	serviceMetaBehavior.HttpGetEnabled = true;
	host.Description.Behaviors.Add(serviceMetaBehavior);
	```

#### Metadata Exchange Endpoint
Exposing the metadata using HTTP-GET has a disadvantage, such that there is no guarantee that other platforms you interact will support it. There is other way of exposing the using special endpoint is called as Metadata Exchange Endpoint (MEX). You can have as many metadata exchange endpoints as you want.

- MEX is a standard service contract for discovering other endpoints 
- Enable by adding `IMetadataExchange` endpoints
- Choose one of the built-in MEX bindings for HTTP,TCP,pipes

You can create the Metadata Exchange Endpoint either in configuration file or code. 
Configuration file:
```xml
<endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange"/>
```

Code:
Steps to create the metadata endpoint are:
- Create the `ServiceMetadataBehavior` object and add to Service host description.
- Create the metadata binding object using `MetadataExchangeBinding`
- Add the endpoint to the service host with address, binding and contract

```csharp
//Create a URI to serve as the base address
Uri httpUrl = new Uri("http://localhost:8090/MyService/SimpleCalculator");
//Create ServiceHost
ServiceHost host = new ServiceHost(typeof(MyCalculatorService.SimpleCalculator), httpUrl);
//Add a service endpoint
host.AddServiceEndpoint(typeof(MyCalculatorService.ISimpleCalculator), new WSHttpBinding(), "");
//Enable metadata exchange
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
host.Description.Behaviors.Add(smb);
Binding mexBinding = MetadataExchangeBindings.CreateMexHttpBinding();
//Adding metadata exchange endpoint
host.AddServiceEndpoint(typeof(IMetadataExchange), mexBinding, "mex");
//Start the Service
host.Open();
```

## Service exceptions
Instruct WCF to include the exception details in faults

- Enable with `[ServiceBehavior]` or the `<serviceDebug>` behavior (includeExceptionDetailInFaults='true')
- Useful for debugging purposes, not recommended in production

`FaultException` class represents an explicit SOAP fault

Always catch exception using try/catch block and throw exception using `FaultException<T>`.

## Operations
WCF will support classical Request-Replay model, along with that it also supports One-Way call(call and forget operation) and callback(service to call back the client)

#### Request-Replay
By default all WCF will operated in the Request-Replay mode. It means that, when client make a request to the WCF service and client will wait to get response from service (till `receiveTimeout`). After getting the response it will start executing the rest of the statement. If service doesn't respond to the service within `receiveTimeout`, client will receive `TimeOutException`.

Apart from `NetPeerTcpBinding` and the `NetMsmqBinding` all other bindings will support request-reply operations.

#### One-Way
In One-Way operation mode, client will send a request to the server and does not care whether it is success or failure of service execution. There is no return from the server side, it is one-way communication.

Client will be blocked only for a moment till it dispatches its call to service.

Client can continue to execute its statement, after making one-way call to server. There is no need to wait, till server execute. Sometime when one-way calls reach the service, they may not be dispatched all at once but may instead be queued up on the service side to be dispatched one at a time, according to the service's configured concurrency mode behavior. If the number of queued messages has exceeded the queue's capacity, the client will be blocked even if it's issued a one-way call. However, once the call is queued, the client will be unblocked and can continue executing, while the service processes the operation in the background.

Limitations:
- The method has no return value (return void)
- Do not support `ref` and `out` parameter

One-way operation can be enabled by setting IsOneWay property to true in Operation contract attribute.
```csharp
[ServiceContract]
public interface IMyService
{
    [OperationContract(IsOneWay=true)]
    void MyMethod(EmployeeDetails emp);
}
```

#### Callback (duplex)
WCF also provides the service to call the client.
- HTTP protocols are connectionless nature, so it is not supported for callback operation. So `BasicHttpBinding` and `WSHttpBinding` cannot be used for this operation.
- WCF support `WSDualHttpBinding` for callback operation.
- All TCP and IPC protocols support Duplex communication. So all these binding will be used for callback operation.

###### Defining and configuring a callback contract
Callback service can be enabled by using `CallbackContrac`t property in the `ServiceContract` attribute. 
```csharp
public interface IMyContractCallback
{
	[OperationContract(IsOneWay=true)]
	void OnCallback();
}

[ServiceContract(CallbackContract = typeof(IMyContractCallback))]
public interface IMyContract
{
	[OperationContract()]
	void MyMethod();
}
```

###### Client Callback Setup
In callback operation client will act as service and service will act as client. So client has to expose a callback endpoint to the service to call. `InstanceContext` is the execution scope of inner most service instance. It provides a constructor that takes the service instance to the host.

```csharp
IMyContractCallback callback = new MyCallback();
InstanceContext cntx = new InstanceContext(callback);

MyServiceClient proxy = new MyServiceClient(cntx);
proxy.MyMethod();
```

The client must use a proxy that will set up the bidirectional communication and pass the callback endpoint reference to the service. This can be achieved by creating the proxy using `DuplexClientBase`.

```csharp
class MyServiceClient : DuplexClientBase, IMyContract
{
	public MyServiceClient(InstanceContext callbackCntx)
		: base(callbackCntx)
	{
	}
	public void MyMethod()
	{
		base.Channel.MyMethod();
	}
}
```

###### Service-Side Callback Invocation
The client-side callback endpoint reference is passed along with every call the client makes to the service, and it is part of the incoming message. The `OperationContext` class provides the service with easy access to the callback reference via the generic method `GetCallbackChannel<T>()`. Service can call the client side callback method using reference to the client side callback instance. The following code shows the callback method invocation.

```csharp
IMyContractCallback callbackInstance = OperationContext.Current.GetCallbackChannel();
callbackInstance.OnCallback();
```

## Events
Events allow the client or clients to be notified about something that has occurred on the service side. The service firing the event is called the publisher, and the client receiving the event is called the subscriber.

- Publisher will not care about order of invocation of subscriber. Subscriber can be executed in any manner.
- Implementation of subscriber side should be short duration. Let us consider the scenario in which you what to publish large volume of event. Publisher will be blocked, when subscriber is queued on previous subscription of the event. These make publishers to put in wait state. It may lead Publisher event not to reach other subscriber.
- Large number of subscribers to the event makes the accumulated processing time of each subscriber could exceed the publisher's timeout
- Managing the list of subscribers and their preferences is a completely service-side implementation. It will not affect the client; publisher can even use .Net delegates to manage the list of subscribers.
- Event should always one-Way operation and it should not return any value

```csharp
public interface IMyEvents
{
	[OperationContract(IsOneWay = true)]
	void Event1();
}

[ServiceContract(CallbackContract = typeof(IMyEvents))]
public interface IMyContract
{
	[OperationContract]
	void DoSomethingAndFireEvent();

	[OperationContract]
	void SubscribeEvent();
}

// service code
[ServiceBehavior(InstanceContextMode = InstanceContextMode.PerCall)]
public class MyPublisher : IMyContract
{
	static Action event1 = delegate { };

	public void SubscribeEvent()
	{
		IMyEvents subscriber = OperationContext.Current.GetCallbackChannel<IMyEvents>();
		event1 += subscriber.Event1;
	}

	public static void FireEvent()
	{
		event1();
	}

	public void DoSomethingAndFireEvent()
	{
		FireEvent();
	}
}

static void Main(string[] args)
{
	Uri httpUrl = new Uri("http://localhost:8090/MyPublisher/");
	ServiceHost host = new ServiceHost(typeof(MyPublisher), httpUrl);
	host.Open();
	Console.WriteLine("Service is Hosted at {0}", DateTime.Now);
	Console.WriteLine("Host is running...Press  key to stop the service.");
	Console.ReadLine();
	host.Close();
}

// client code
class EventServiceClient : DuplexClientBase<IMyContract>, IMyContract
{
	public EventServiceClient(InstanceContext eventCntx)
		: base(eventCntx)
	{
	}

	public void DoSomethingAndFireEvent()
	{
		base.Channel.DoSomethingAndFireEvent();
	}

	public void SubscribeEvent()
	{
		base.Channel.SubscribeEvent();
	}
}

class MySubscriber : IMyEvents
{
	public void Event1()
	{
		Console.WriteLine("Event is subscribed from the service at {0}", DateTime.Now);
	}
}

static void Main(string[] args)
{
	IMyEvents event = new MySubscriber();
	InstanceContext eventCtx = new InstanceContext(event);
	EventServiceClient proxy = new EventServiceClient(eventCtx);
	Console.WriteLine("Client subscribe the event from the service at {0}", DateTime.Now);
	proxy.SubscribeEvent();
	Console.WriteLine("Client call operation which will fire the event");
	proxy.DoSomethingAndFireEvent();
	Console.ReadLine();
}
```

# Programming Clients

In order for client application to consume WCF service, first it needs to retrieve the service's metadata. The service will expose the metadata typically in the form of a WSDL definition. The client will retrieve that WSDL definition and run it through a client-side metadata import tool. By doing that in WCF that will produce client-side endpoint definitions that represents the information needed to communicate with that service through one of its endpoints. Client, once it has those endpoint definitions, can then choose an endpoint and construct a channel based on that endpoint. Then it can begin sending messages to the service through that channel by simply making method calls on the channel instance. 

## Retrieve endpoint definitions
Client can automatically generate endpoint definitions from WSDL

- Generate equivalent contract definitions (code)
- Generate equivalent endpoint definitions (configuration)

There are different ways of creating the proxy class for the client.

1. `SvcUtil.exe` is a metadata import tool for producing WCF client code
	- Downloads WSDL and generates WCF code and configuration
	- Supports MEX retrieval over HTTP,TCP,pipes
2. `Add Service Reference` feature in VS
	- So you don't have to run SvcUtil.exe directly in most cases
	- Provides most of same functionality
	- Generates code/configuration and adds it to the project 
3. Implementing `ClientBase<T>` class

Of these three methods, Implementing `ClientBase<T>` is the best practice. If you are using rest two method, we need to create proxy class every time when we make changes in Service implementation. But this is not the case for `ClientBase<T>`. It will create the proxy only at runtime and so it will take care of everything.

## Programming WCF channels
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

## Configure client channels:

- Client channels can be configured via the binding & behaviors
- Key things to configure on the binding:
	- `sendTimeout` value - influence when the TimeoutException occurs
	- Transport/protocol specific settings

## Handling exceptions
Client channels need to be prepared for two main exception types:

- `CommunicationException` 
	- Various runtime communication errors
	- `FaultException`'s thrown by operations
- `TimeoutException`
	- `sendTimeout` limit exceeded
	- Thrown by underlying transport channel 

## Invoking services asynchronously
- Use the `/async /targetClientVersion:Version35` switch when generating the client code (check 'Generate asynchronous operation' in 'Configure Service Reference' menu item)
- This produces an XXXAsync method + an event for each operation
- Simply hook up event and call the asynchronous method

## Sharing service contract assemblies
Tell import tools to reuse types in referenced assemblies.

- Use `/reference` (SvcUtil.exe) or the Service Reference Settings (or just check `Reuse types in referenced assemblies`). Then the tool only generates config, not contracts.

## Programming MEX
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

## Web Programming Model Comparison
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

## REST Support in WCF
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

```csharp
[ServiceContract]
public interface IEmployeeService
{
	[WebGet(UriTemplate = "Employee", ResponseFormat = WebMessageFormat.Json)]
	[OperationContract]
	List<Employee> GetAllEmployeeDetails();

	[WebGet(UriTemplate = "Employee?id={id}", ResponseFormat = WebMessageFormat.Json)]
	[OperationContract]
	Employee GetEmployee(int Id);

	[WebInvoke(Method = "POST", UriTemplate = "EmployeePOST", ResponseFormat = WebMessageFormat.Json)]
	[OperationContract]
	void AddEmployee(Employee newEmp);

	[WebInvoke(Method = "PUT", UriTemplate = "EmployeePUT", ResponseFormat = WebMessageFormat.Json)]
	[OperationContract]
	void UpdateEmployee(Employee newEmp);

	[WebInvoke(Method = "DELETE", UriTemplate = "Employee/{empId}", ResponseFormat = WebMessageFormat.Json)]
	[OperationContract]
	void DeleteEmployee(string empId);
}
```


