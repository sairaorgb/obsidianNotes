**Cross origin resource sharing (CORS)**

- `origin` is protocol + hostname + port of the page making request from browser.
- cross origin requests are blocked by browser by default , and are dealt in two ways
	- Simple request - (GET, HEAD, POST with only `text/plain`, `application/x-www-form-urlencoded`, or `multipart/form-data` and no custom headers) - Browser sends the request for which the server should send cors headers along with response - if fields match , browser will pass to frontend or else it dumps response.
	- Non simple requests - (GET, HEAD, POST with only `text/plain`, `application/x-www-form-urlencoded`, or `multipart/form-data` and no custom headers) - Browser sends preflight options request that'll prompt server to send a cors fields and then the original request is sent. Actual response should also contain cors fields.
- cors middleware can be used with express to send cors headers in response that include 
``` js
Access-Control-Allow-Origin: <origin>
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```
``` js
# CORS middleware 
import cors from "cors";
app.use(cors({
  origin: "http://localhost:3000",
  methods: ["GET","POST","PUT","DELETE"],
  allowedHeaders: ["Content-Type","Authorization"],
  credentials: true,
}));

```