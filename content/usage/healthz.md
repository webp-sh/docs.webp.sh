# Healthz

There is a `/healthz` endpoint that can be used to check if the server is up and running. You can use this endpoint for liveness probe.

It returns a `200` status code if the server is up and running.


```
curl  localhost:3333/healthz -v
*   Trying 127.0.0.1:3333...
* Connected to localhost (127.0.0.1) port 3333 (#0)
> GET /healthz HTTP/1.1
> Host: localhost:3333
> User-Agent: curl/8.0.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: WebP Server Go
< Date: Fri, 15 Dec 2023 03:16:08 GMT
< Content-Type: text/plain; charset=utf-8
< Content-Length: 34
< Etag: W/"34-1945155614"
< 
* Connection #0 to host localhost left intact
WebP Server Go up and running!🥳%              
```