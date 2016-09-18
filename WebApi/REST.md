## Web Architecture
The Web is built around three core concepts: resources, URIs, and representations.
A resource has a URI that identifies it and that HTTP clients will use to find it. A representation is data that is returned from that resource. Also related and significant is the media type, which defines the format of that data.

## HTTP
#### HTTP Methods
#### Intermediaries
Intermediaries can improve or enhance communication.
- Proxy
- Gateway
- Tunnel

#### Headers
- Message headers
- Request headers
- Response headers
- Representation headers

#### HTTP Status Code
#### Content Negotiation
HTTP servers often have multiple ways to represent the same resource. The representations can be based on a variety of factors. The dance of selection between client and server is known as content negotiation.

#### Cache
Caches will try to handle only requests that use a cacheable method. All other requests will be automatically forwarded to the origin server. The cache will also forward to the origin server requests that are cacheable, but that are either not present in the cache or expired.

#### Authentication
HTTP provides an extensible framework for servers that allows them to protect their resources and allows clients to access them through authentication.
Authentication Schemes:
- Basic
- Digest

Additional Authentication Schemes
- AWS Authentication
- OAuth 2.0

## RPC (Remote Procedure Call)
RPC style API treats HTTP as a transport protocol for invoking functions running on a remote server. RPC API exposes a single URI and one HTTP method (POST), with different message types corresponding to different methods on the remote object. The response status is in the body itself, not in the status code.

## REST
Resource oriented. API exposes one or more resources, which each support one or more HTTP methods. These APIs offer rich interactions over HTTP, supporting capabilities like caching and content negotiation.

## REST vs. SOAP RPC

#### SOAP
SOAP services are not web-friendly, they are not easily consumable from HTTP clients, such as web browsers. A SOAP request has to be properly encoded in a SOAP message format. The client has to have access to a WSDL file, which describes the actions available on the service, and also has to know how to construct the message. SOAP services require all interactions to be via HTTP POST, so the response are not cacheable. SOAP services do not allow one to access HTTP headers, which severely limits clients from benefitting from features of HTTP like optimistic concurrency and content negotiation.

#### Advantages of SOAP
- Custom security protocol via WS Security
- Distributed Transactions via WS Atomic Transaction
- Retry logic via WS Reliable Messaging
- Duplex communication via WsDualHttpBinding and NetTcpBinding
- Formal service contracts (using WSDL) and strong typing
- Built-in stateful operations, support conversational state management

#### REST
RESTful services are browser-friendly. Can be accessed from a range of client. Support standard HTTP methods. Support browser-friendly formats. Support browser-friendly authentication.

#### Advantages of REST
- Simple: easy to understand, uses HTTP and basic CRUD operations
- Flexible Data Formats: REST supports many data formats, but the predominant use of JSON means better support for browser clients. REST makes efficient use of bandwidth, as it’s much less verbose than SOAP.
- Cross-platform client support
- Choices for backend 
- Caching: designed to be stateless, and REST reads can be cached for better performance and scalability
- Scalability
HTTP specification lets you spot security via certain HTTP headers, so you can leverage this to make your API secure.

## Idempotent and/or Safe Methods
Safe methods are HTTP methods that do not change resources. It won't change the resource representation. It is still possible that safe methods do change things on a server or resource, but this should not reflect in a different representation. Safe methods can be cached.
Be careful when dealing with safe methods: if a seemingly safe method like GET will change a resource, it might be possible that any middleware client proxy systems between you and the server, will cache this response. Another client who wants to change this resource through the same URL (like: http://example.org/api/article/1234/delete), will not call the server, but return the information directly from the cache. Non-safe (and non-idempotent) methods will never be cached by any middleware proxies.

An idempotent HTTP method will result in the same state on the server no matter how many times that same request is executed. This only applies to the result, not the resource itself.
Idempotency is important in building a fault-tolerant API. Suppose a client wants to update a resource through POST. Since POST is not a idempotent method, calling it multiple times can result in wrong updates. What would happen if you sent out the POST request to the server, but you get a timeout. Is the resource actually updated? By using idempotent methods, we do not have to answer this question, but we can safely resend the request until we actually get a response back from the server.

Consider the following examples:

```
a = 4;
a++;
```

The first example is idempotent: no matter how many times we execute this statement, a will always be 4. The second example is not idempotent. Since both examples are changing the value of a, both are non-safe methods.

| HTTP Method | Idempotent | Safe | Cachable |
| --- | --- | --- | --- |
| OPTIONS | Yes | Yes | No |
| HEAD | Yes | Yes | Yes |
| GET | Yes | Yes | Yes |
| PUT | Yes | No | No |
| DELETE | Yes | No | No |
| POST | No | No | No |
| PATCH | Yes | No | No |
| TRACE | Yes | Yes | No |

## PUT vs POST
| HTTP Verb | CRUD |
| --- | --- |
| POST | Create |
| GET | Read |
| PUT | Update |
| DELETE | Delete  |

`GET`与`DELETE`对应的操作是很明确的，但选取create和update对应的HTTP方法时要取决于幂等性(Idempotent)。

[https://stormpath.com/blog/put-or-post](To PUT or POST?)

#### Creates
- Use `POST` to create resources when you do not know the resource identifier (location). With `POST` creates, it is best practice to return the status of “201 Created” and the location of the newly created resource, since its location was unknown at the time of submission.
- Use `PUT` when you allow the client to specify the resource identifier of the newly created resource. But remember, since `PUT` is idempotent, you must send all possible values.

#### Updates
- You can use `POST` to send either all available values or just a subset of available values (full update)
- If you want to use `PUT` to update a resource, it must be a full resource update; you MUST send all attribute values in a PUT request to guarantee idempotency.

With `PUT` requests you MUST send all the available properties/values, not just the ones you want to change. Otherwise, it's not idempotent. Because other requests might have changed the server state of the resource in between our two requests.

Note that idempotency has a strong correlation to being cacheable by HTTP caching servers, and therefore `POST` requests are generally not cached. If you are ok with this caching side effect, you can use `POST` for both full and partial updates.

`POST` is non-idempotent method. The HTTP specification is very generic about it, and basically states it to be a ‘server processing directive’. This means it is ‘safe’ to do whatever you want during a `POST` request.

## API Styles
- RPC oriented
- Resource oriented
- HTTP verbs
- Hypermedia: rel, href

## REST Constraints

Rest is s style, not a technology, not a framework, not a design pattern. There are many flavors of RESTful systems, they all manifest themselves in a set of constraints.

REST define the following constraints:
- Client-server
- Stateless
- Cache
- Uniform interface
- Identification of resources
- Self-descriptive messages
- Manipulation of resource through representations
- Hypermedia as the engine of application state
- Layered system
- Code on demand

http://www.ruanyifeng.com/blog/2011/09/restful.html
http://restcookbook.com/

