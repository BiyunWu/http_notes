#### HTTP stands for Hypertext Transfer Protocol
  Prerequisites:

  * Homebrew
  * Python 3
  * ncat (A aprt of the Nmap network testing toolkit)
  > Press `Ctrl` + `C` to exit ncat

#### Initialize a server through Python
  1. `cd` to the server directory
  2. Use command `python3 -m http.server port_code` to set up the server side: for example, `python3 -m http.server 8000`
  3. Visit through <http://localhost:8000/> in browser.
  > If there is a `index.html` file in the server directory, the browser will automatically display it as a website instead of the server's hierachy tree.

#### URI: Uniform Resource Identifier; URL: Uniform Resource Locator

  >For example, URI: `https://en.wikipedia.org/wiki/Fish`

  >This URI has three visible parts, separated by a little bit of punctuation:

  >* `https` is the **scheme**;
   * `en.wikipedia.org` is the **hostname**;
   * and `/wiki/Fish` is the **path**.

  URI schemes: http, https, file, ftp, etc. [Full list](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)

#### Relative URI Reference
  `<a herf="building.jpg">Building</a>`

#### Other URI Parts
> * <https://en.wikipedia.org/wiki/Oxygen>
  * <https://en.wikipedia.org/wiki/Oxygen#Discovery>

  `#` sign is a **fragment** that reperents the `id` of the element in *.html* file. `#Discovery` does not be sent to the sever, but it allows the browser to locate the specified content in the web page.

> <https://www.google.com/search?q=fish>

  The `?q=fish` is a **query** part of the URI. This does get sent to the server.

  [More aprts of a URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

#### Hostname
  Hostnames are always be interpreted into IP address through DNS(Domain Name Service).

#### Localhost
  * IPv4: `127.0.0.1`
  * IPv6: `::1`
  > which is short for `0000:0000:0000:0000:0000:0000:0000:0001`

#### Ports
  * HTTP: `80`
  * HTTPS: `443`

  All information computers send and recieve is split up into **packets**. Ports distinguish which program running on a computer to receive the packets. It is similar to a server recognizes a client through IP address.

#### HTTP Get Request
  Send `Get` request manually

  * Use `python3 -m http.server 8000` to set up the server side, then in a seperated terminal, use `ncat 127.0.0.1 8000` to access the server.
  * Type the following two lines of code, and then qiuckly press `Enter` key for twice.

  `GET / HTTP/1.1`

  `Host: localhost`

#### HTTP Responses

```
  ncat google.com 80
  GET / HTTP/1.1
  Host: google.com

  HTTP/1.1 301 Moved Permanently                // Status line
  Location: http://www.google.com/              // Header starts
  Content-Type: text/html; charset=UTF-8
  Date: Sat, 04 Nov 2017 21:32:52 GMT
  Expires: Mon, 04 Dec 2017 21:32:52 GMT
  Cache-Control: public, max-age=2592000
  Server: gws
  Content-Length: 219
  X-XSS-Protection: 1; mode=block
  X-Frame-Options: SAMEORIGIN                   // Header ends

  <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">     // Response Body
  <TITLE>301 Moved</TITLE></HEAD><BODY>
  <H1>301 Moved</H1>
  The document has moved
  <A HREF="http://www.google.com/">here</A>.
  </BODY></HTML>
```
  In the status line, the number `200` or `301` are HTTP **status code**.

  >There are dozens of different status codes. The first digit of the status code indicates the general success of the request. As a shorthand, web developers describe all of the codes starting with **2** as **"2xx"** codes, for instance — the x's mean "any digit".

  >* **1xx — Informational. **The request is in progress or there's another step to take.
  >* **2xx — Success!** The request succeeded. The server is sending the data the client asked for.
  >* **3xx — Redirection.** The server is telling the client a different URI it should redirect to. The headers will usually contain a **Location** header with the updated URI. Different codes tell the client whether a redirect is permanent or temporary.
  >* **4xx — Client error.** The server didn't understand the client's request, or can't or won't fill it. Different codes tell the client whether it was a bad URI, a permissions problem, or another sort of error.
  >* **5xx — Server error.** Something went wrong on the server side.

  A header is made up of a keyword, a colon and a value.

  `Content-Type` header delivers the information that what type of information the server is sending.

  **Experiment 1**

  * In terminal:
  > ncat -l 9999

  * In broswer: http://localhost:9999/
  * In terminal:
  > `HTTP/1.1 307 Temporary Redirect` <br>
    `Location: https://www.apple.com/`<br>
    Quick press `Enter` twice.

  **Experiment 2**

  * In terminal:
  > ncat -l 9999

  * In broswer: http://localhost:9999/
  * In terminal:
  > `HTTP/1.1 201 OK` <br>
    `Content-type: text/plain` <br>
    `Content-length: 15` <br>
    <br>
    `This is a test.` <br>
    <br>
    Quick press `Enter` twice.

  Note: the response header is seperated form the response body by a blank line.
