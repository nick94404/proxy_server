# proxy_server
# Proxy Server

This is a Proxy Server for Node.js submitted as the [pre-work](http://courses.codepath.com/snippets/intro_to_nodejs/prework) requirement for CodePath.

Time spent: 10 hours

Completed:

* [x] Required: Requests to port `8000` are echoed back with the same HTTP headers and body
* [x] Required: Requests/reponses are proxied to/from the destination server
* [x] Required: The destination server is configurable via the `--host`, `--port`  or `--url` arguments
* [x] Required: The destination server is configurable via the `x-destination-url` header
* [x] Required: Client requests and respones are printed to stdout
* [x] Required: The `--logfile` argument outputs all logs to the file specified instead of stdout
* [] Optional: The `--exec` argument proxies stdin/stdout to/from the destination program
* [] Optional: The `--loglevel` argument sets the logging chattiness
* [] Optional: Supports HTTPS
* [] Optional: `-h` argument prints CLI API

Walkthrough Gif:
proxyserver.gif


## Starting the Server

```bash
npm start
or
nodemon -- index.js

## Features

### Echo Server:

```bash
curl -v -X POST http://127.0.0.1:8000 -d "hello self" -H "x-asdf: yodawg"
* About to connect() to 127.0.0.1 port 8000 (#0)
*   Trying 127.0.0.1...
* connected
* Connected to 127.0.0.1 (127.0.0.1) port 8000 (#0)
> POST / HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8} zlib/1.2.5
> Host: 127.0.0.1:8000
> Accept: */*
> x-asdf: yodawg
> Content-Length: 10
> Content-Type: application/x-www-form-urlencoded
>
* upload completely sent off: 10 out of 10 bytes
< HTTP/1.1 200 OK
< user-agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8} zlib/1.2.5
< host: 127.0.0.1:8000
< accept: */*
< x-asdf: yodawg
< content-length: 10
< content-type: application/x-www-form-urlencoded
< Date: Fri, 16 Sep 2016 05:29:47 GMT
< Connection: keep-alive
<
* Connection #0 to host 127.0.0.1 left intact
hello self* Closing connection #0

### Proxy Server:

Port 9000 will proxy to the echo server on port 8000.
```bash
curl -v http://127.0.0.1:9000/asdf -d "hello proxy"
* About to connect() to 127.0.0.1 port 9000 (#0)
*   Trying 127.0.0.1...
* connected
* Connected to 127.0.0.1 (127.0.0.1) port 9000 (#0)
> POST /asdf HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8} zlib/1.2.5
> Host: 127.0.0.1:9000
> Accept: */*
> Content-Length: 11
> Content-Type: application/x-www-form-urlencoded
>
* upload completely sent off: 11 out of 11 bytes
< HTTP/1.1 200 OK
< host: 127.0.0.1:8000
< user-agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8} zlib/1.2.5
< accept: */*
< content-length: 11
< content-type: application/x-www-form-urlencoded
< connection: close
< date: Fri, 16 Sep 2016 05:31:34 GMT
<
* Closing connection #0
hello proxy


### Configuration:

#### CLI Arguments:

The following CLI arguments are supported:

##### `--host`

The host of the destination server. Defaults to `127.0.0.1`.

##### `--port`

The port of the destination server. Defaults to `80`, or `8000` for 127.0.0.1,  when a host is not specified.

##### `--url`

A single url that overrides the above. E.g., `http://www.google.com`

##### `--logfile`

Specify a file path to redirect logging to.

#### Headers

The follow http header(s) are supported:

##### `x-destination-url`

Specify the destination url on a per request basis. Overrides and follows the same format as the `--url` argument.
