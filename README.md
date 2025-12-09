# Build it using C#

[![progress-banner](https://backend.codecrafters.io/progress/http-server/3357626b-124f-4d56-9090-900013a94a0c)](https://app.codecrafters.io/users/codecrafters-bot?r=2qF)

---

This repository contains a C# implementation of an HTTP/1.1 server. It is capable of handling multiple client connections concurrently and supports various HTTP features, including request parsing, header processing, file serving (from a specified directory), and Gzip content compression. This project was developed based on the CodeCrafters "Build your own HTTP server" challenge.

## Implemented Features

- [x] Bind to a port
- [x] Respond with 200
- [x] Extract with 200
- [x] Extract URL path
- [x] Respond with body
- [x] Read header
- [x] Concurrent connections
- [x] Return a file
- [x] Read request body
- [x] Compression headers

## Usage Examples

### Root Path

Responds with a simple 200 OK for the root path (`/`).

```http
GET / HTTP/1.1
Host: localhost:4221

HTTP/1.1 200 OK
Connection: keep-alive
```

### Echo Service

The `/echo/{message}` endpoint returns the `{message}` part of the URL path in the response body. It supports Gzip compression if the client includes `Accept-Encoding: gzip` in the request headers.

**Without Gzip compression:**
```http
GET /echo/hello-world HTTP/1.1
Host: localhost:4221

HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 11
Connection: keep-alive

hello-world
```

**With Gzip compression:**
```http
GET /echo/hello-world HTTP/1.1
Host: localhost:4221
Accept-Encoding: gzip

HTTP/1.1 200 OK
Content-Encoding: gzip
Content-Type: text/plain
Content-Length: 30
Connection: keep-alive

[compressed content of "hello-world"]
```

### User-Agent Information

The `/user-agent` endpoint returns the value of the `User-Agent` header from the request body.

```http
GET /user-agent HTTP/1.1
Host: localhost:4221
User-Agent: (Windows NT 10.0; Win64; x64)

HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 41
Connection: keep-alive

 (Windows NT 10.0; Win64; x64)
```

### File Operations

The server can serve files from a directory specified using the `--directory <path>` command-line argument. The `/files/{filename}` endpoint allows for creating files (via POST) and retrieving files (via GET).

**Creating a file (POST):**
The content of the request body will be written to the specified file.
```http
POST /files/tmp.txt HTTP/1.1
Host: localhost:4221
Content-Length: 18

This is a test file

HTTP/1.1 201 Created
Connection: keep-alive
```

**Retrieving a file (GET):**
The content of the specified file will be returned in the response body.
```http
GET /files/tmp.txt HTTP/1.1
Host: localhost:4221

HTTP/1.1 200 OK
Content-Type: application/nest-kit
Content-Length: 18
Connection: keep-alive

This is a test file
```

**Attempting to retrieve a non-existent file (GET):**
```http
GET /files/nonexistent.txt HTTP/1.1
Host: localhost:4221
TP/1.1 404 Not Found
Connection: keep-alive
```
