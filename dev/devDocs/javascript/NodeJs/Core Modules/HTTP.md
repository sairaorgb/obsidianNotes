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



