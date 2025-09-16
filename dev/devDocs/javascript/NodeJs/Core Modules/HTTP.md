- A core node module that lets you create HTTP servers and clients.

**workflow of a request/response cycle**

- client send request -> Node's server parses into an IncomingMessage
- Server code inspects request data (`req.method`, `req.url`).
- Server writes a response using `ServerResponse`
- Connection may close, or keep alive (HTTP/1.1 default).

#### Building Blocks

###### Creating and listening on a HTTP server 

createServer Takes a callback   (req , res) => { } where
- req is http.IncomingMessage ( an obj with request data )
- res is http.ServerResponse ( obj used to send back )
``` js
// Import the HTTP module  
const http = require('http');  
  
// Create a server object  
const server = http.createServer((req, res) => {  
  // Set the response HTTP header with HTTP status and Content type  
  res.writeHead(200, { 'Content-Type': 'text/plain' });  
  
  // Send the response body as 'Hello, World!'  
  res.end('Hello, World!\n');  
});  
  
// Define the port to listen on const PORT = 3000;  
// Start the server and listen on the specified port  
server.listen(PORT, 'localhost', () => {  
  console.log(`Server running at http://localhost:${PORT}/`);  
});
```


-  `res.writeHead` method is used to set status codes and response headers and `res.end` is used for writing body of response.
###### HTTP Headers
 
 Response Headers :
-  `Content-Type`: Specifies the media type of the content (e.g., text/html, application/json)
- `Content-Length`: The length of the response body in bytes
- `Location`: Used in redirects (with 3xx status codes)
- `Set-Cookie`: Sets HTTP cookies on the client
- `Cache-Control`: Directives for caching mechanisms
- `Access-Control-Allow-Origin`: For CORS support

Status Codes:
- 200	OK 
- 201	Created	
- 301	Moved Permanently	
- 400	Bad Request	
- 401	Unauthorized	
- 403	Forbidden	
- 404	Not Found	
- 500	Internal Server Error

## URL 

URL is basically a postal address for the internet.

``` bash
http://localhost:3000/users/42?active=true&sort=desc
```

- protocol : `http://` or `https://`
- Host : `localhost` or `example.com`
- port : 3000 ( if not default )
- pathname: `/users/42`
- Query string : `?active=true & sort=desc`
- Hash: `#section1` (not sent to server, browser-only)

- HTTP module doesn’t give you the protocol, host, or port — just the **path + query** .
- `req.method` - Gives GET, POST, UPDATE, DELETE ( req is a HTTP.IncomingMessage Object )
- `req.url` - Gives pathname and query
``` js
const http = require('http');

const server = http.createServer((req, res) => {
  console.log(req.url);       // /users?active=true
  res.end('Hello, world!');
});

server.listen(3000);
```

**Parsing the URL**
``` js
const url = require('url');

http.createServer((req, res) => {
const parsed = url.parse(req.url, true); // true → parse querystring into object
  console.log(parsed.pathname); // "/users"
  console.log(parsed.query);    // { active: 'true' }
  res.end('URL parsed!');
}).listen(3000);
```

**Creating URL object**
- URL object encapsulates information in a parsed way .
- `querystring` or `URLSearchParams` can be used to parse the query string into an object.
``` js
const { searchParams } = new URL(req.url, `http://${req.headers.host}`);
```

``` js
{
  href: "http://localhost:3000/users?active=true",
  origin: "http://localhost:3000",
  pathname: "/users",
  search: "?active=true",
  searchParams: URLSearchParams { active: "true" }
}

// Query Parameters
console.log(searchParams.get('id')); // "123"
console.log(searchParams.get('active')); // "true"
```



