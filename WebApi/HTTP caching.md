For HTTP caching to work, client and server need to work in tandem. Server needs to return with each resource, a Cache-Control header, an ETag or a LastModified and the client needs to use these parameters in its future conditional requests with If-Modified-Since or If-None-Match. 

Server-side caching according to HTTP spec: includes inspecting headers, generating ETag and adding headers, responding to conditional GET and PUT, etc.

ASP.NET Web API exposes full goodness of the HTTP spec and caching can be implemented as a message handler.

## Background

HTTP caching is not about putting an object in memory as we do with HttpRuntime.Cache. In fact server does not store anything, it only tells the client (or mid-stream cache or proxy servers) what can be cached, how long and validates the caching.

Basically, HTTP provides semantics and mechanism for the origin server, client and mid-stream proxy/cache servers to effectively reduce traffic by validating the version of the resource they have against the server and retrieve the resource only if it has changed. This process is usually referred to as cache validation.

In HTTP 1.0, server would return a LastModified header with the resource. A client/user agent could use this value and send it in the If-Modified-Since header in the subsequent GET requests. If the resource was not changed, server would respond with a 304 (Not Modified) otherwise the resource would be sent back with a new LastModified header. This would be also useful in PUT scenarios where a client sends a PUT request to update a resource only if it has not changed: a If-Unmodified-Since header is sent. If the resource has not changed, server fulfils the request and sends a 2xx response (usually 202 Accepted) otherwise a 412 (Precondition Failed) is sent.

For many reasons it was felt that this mechanism was not adequate. In HTTP 1.1, ETag (Entity Tag) was introduced which is an opaque Id in the format of a quoted string that is returned with the resource. ETags can be strong (refer to RFC 2616 for more info) or weak in which case they start with w/ such as w/"12345". ETag works like a version ID for a resource - if two ETags (for the same resource) are equal, it means those two versions of the resource are the same.

ETag for the same resource could be different according to various headers. For example a client can send an Accept-Language header of de-DE or en-GB and server would send different ETags. Server can define this variability for each resource by using Vary header.

Cache validation for GET and PUT requests are similar but instead will use If-Match (for PUT) and If-None-Match (for GET).

For example, we make request and get back this response (or similar; some headers removed and body truncated for clarity):
```
HTTP/1.1 200 OK
ETag: "54e9a75f2dbb4edca672f7a2c4a73dca"
Vary: Accept
Cache-Control: no-transform, must-revalidate, max-age=604800, private
Last-Modified: Thu, 21 Jun 2012 23:35:46 GMT
Content-Type: application/json; charset=utf-8

[{"Id":1,"Make":"Vauxhall","Model":"Astra","BuildYear":1997,"Price":175.0....
```
The client has to do some work to do. It has to use If-None-Match with the ETag to conditionally ask for the resource: if it matches server, it will get back 304 but if not, server will return the new resource:
```
GET http://localhost:8031/api/Cars HTTP/1.1
User-Agent: Fiddler
Host: localhost:8031
If-None-Match: "54e9a75f2dbb4edca672f7a2c4a73dca"
```
Here we get back 304 (Not modified) as expected:
```
HTTP/1.1 304 Not Modified
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
ETag: "54e9a75f2dbb4edca672f7a2c4a73dca"
Server: Microsoft-IIS/8.0
Date: Sun, 24 Jun 2012 07:34:29 GMT
```

https://github.com/aliostad/CacheCow
http://byterot.blogspot.co.uk/2012/05/aspnet-web-api-series-messagehandler.html
http://byterot.blogspot.co.uk/2012/05/aspnet-web-api-messagehandler-real.html
