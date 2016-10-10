For HTTP caching to work, client and server need to work in tandem. Server needs to return with each resource, a Cache-Control header, an ETag or a LastModified and the client needs to use these parameters in its future conditional requests with If-Modified-Since or If-None-Match. 

Server-side caching according to HTTP spec: includes inspecting headers, generating ETag and adding headers, responding to conditional GET and PUT, etc.

ASP.NET Web API exposes full goodness of the HTTP spec and caching can be implemented as a message handler.

HTTP caching is not about putting an object in memory as we do with HttpRuntime.Cache. In fact server does not store anything, it only tells the client (or mid-stream cache or proxy servers) what can be cached, how long and validates the caching.

Basically, HTTP provides semantics and mechanism for the origin server, client and mid-stream proxy/cache servers to effectively reduce traffic by validating the version of the resource they have against the server and retrieve the resource only if it has changed. This process is usually referred to as cache validation.

In HTTP 1.0, server would return a LastModified header with the resource. A client/user agent could use this value and send it in the If-Modified-Since header in the subsequent GET requests. If the resource was not changed, server would respond with a 304 (Not Modified) otherwise the resource would be sent back with a new LastModified header. This would be also useful in PUT scenarios where a client sends a PUT request to update a resource only if it has not changed: a If-Unmodified-Since header is sent. If the resource has not changed, server fulfils the request and sends a 2xx response (usually 202 Accepted) otherwise a 412 (Precondition Failed) is sent.

For many reasons (including the fact that HTTP dates do not have milliseconds) it was felt that this mechanism was not adequate. In HTTP 1.1, ETag (Entity Tag) was introduced which is an opaque Id in the format of a quoted string that is returned with the resource. ETags can be strong (refer to RFC 2616 for more info) or weak in which case they start with w/ such as w/"12345". ETag works like a version ID for a resource - if two ETags (for the same resource) are equal, it means those two versions of the resource are the same.

ETag for the same resource could be different according to various headers. For example a client can send an Accept-Language header of de-DE or en-GB and server would send different ETags. Server can define this variability for each resource by using Vary header.

Cache validation for GET and PUT requests are similar but instead will use If-Match (for PUT) and If-None-Match (for GET).

https://github.com/aliostad/CacheCow
http://byterot.blogspot.co.uk/2012/05/aspnet-web-api-series-messagehandler.html
http://byterot.blogspot.co.uk/2012/05/aspnet-web-api-messagehandler-real.html
