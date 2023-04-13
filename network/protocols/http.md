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


## Headers

### User-Agent

#### History of User-Agent sniffing

Mosaic, very first browser in 1993, has User-Agent "Mozaic/0.9".
Next, Mozilla come. And its name changed to Netscape.
But, its UserAgent still have "Mozilla" name.
Microsoft created "Internet Exploer". But some server judge whether Mozaic or Mozzila by checking User-Agent.
So, Internet Exploer have User-Agent such as "Mozilla/2.0 (compatible; MSIE 3.02; Windows 95)".
These kind of checking User-Agent is called User-Agent sniffing.

#### How to judge User-Agent

User-Agent for modern browsers start with "Mozilla/5.0" for historical reason.
User-Agent for modern browsers have following format.

`"Mozilla/5.0 ([OS version]) [Rendering Engine version] [Broser version and some dummy browser names]"`
- `[OS version]`
  - Windows NT 10.0; Win64; x64
  - iPhone; CPU iPhone OS 12_0 like Mac OS X
  - Macintosh; Intel Mac OS X 10_13_6
- `[Rendering Engine]`
  - AppleWebKit/537.36 (KHTML, like Gecko)
    - KHTML and like Gecko is for compatability
- `[Browser version and some dummy browser names]`
  - Chrome/69.0.3497.100 Safari/537.36


Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Safari/537.36

We only consider following browser:
- Chrome
- Edge
- Safari
- Firefox

By following in order, you can judge true user agent.

1. If the User-Agent contains "Edge", it is Edge. (Edge sometimes contain "Chrome".)
2. If the User-Agent contains "Chrome", it is chrome. (Chrome sometimes contain "Safari".)
3. Firefox contains "Firefox", and Mac contains "Safari".


#### Tips

- User-Agent is currenlty not one of forbidden headers, but in some implementation (such as Chrome), you cannot overwrite User-Agent header.


## References

- History of HTTP: https://qiita.com/hirooka0527/items/13767855358f83db5e02
- HTTP/0.9: https://www.w3.org/DesignIssues/HTTP0.9Summary.html
- SPDY: https://www.chromium.org/spdy/spdy-whitepaper/
- UserAgent history: https://ics.media/entry/200729
- Forbidden headers: https://developer.mozilla.org/ja/docs/Glossary/Forbidden_header_name
