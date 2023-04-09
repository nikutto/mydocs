# HTTP

Write what I know.

## History

### HTTP/0.9

Since 1990.

- Only GET method is available
- Just return HTML

### HTTP/1.0

Since 1996.

- You can specify several methods.
  - GET
  - POST
  - PUT
  - DELETE
  - etc...
- You can use other document type.
- You can use redirect

### HTTP/1.1

Since 1997.

Some techinical updates.
- Chunk encoding
- Keep-Alive

### SPDY

Since 
To improve latency.
Problems of HTTP 1.1 described at https://www.chromium.org/spdy/spdy-whitepaper/.
- Single request per connection.
- Client-initiated requests
- Uncompressed metadatas.
- Redundant headers.

SPDY adds a session layer atop of SSL layer (Therefore, SSL/TLS is required.)


### HTTP/2.0

Since 2015.
Inspired by SPDY.
- Binary based (HTTP/1.1 was text based.)
- Multiplexing (Derived from SPDY)
- Heaer compression (Derived from SPDY)
- Server push (Derived from SPDY)
- Only supported over SSL/TLS

### HTTP/3

Application protocol over QUIC (UDP based transport layer protocol).
In developper mode, this protocol is written as h3.
- Solve problem of Head-of-Line Blocking. (Since L4 layer is QUIC.)

## References

- History of HTTP: https://qiita.com/hirooka0527/items/13767855358f83db5e02
- HTTP/0.9: https://www.w3.org/DesignIssues/HTTP0.9Summary.html
- SPDY: https://www.chromium.org/spdy/spdy-whitepaper/