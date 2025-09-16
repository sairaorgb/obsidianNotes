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




