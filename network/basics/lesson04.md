## HTTP RFC
HTTP application layer protocol specified in [HTTP RFC](https://datatracker.ietf.org/doc/html/rfc7231). We will discuss some of the topics covered in the RFC.

Anyone take RFC and write software meeting RFC specification. Oldest HTTP Server is Apache Foundation [Apache Httpd](https://httpd.apache.org/) can download and install on your laptop. If your laptop IP is 192.168.29.100 you can access page by http://192.168.29.100/yourpage.html

We are familiar with browsers being http clients, there are multiple other ways you can access and one very popular is [curl](https://curl.se/)


Usage of curl similar to browser in simple method

```
curl https://curl.se
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html lang="en">
<head>
<title>curl</title>
```
http has multiple request methods, as
GET (most command)
HEAD
POST
PUT
DELETE
CONNECT
OPTIONS
TRACE

The Head method is similar to GET but only transfer the status line  and header section.

```
curl -I  https://curl.se
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 8018
Server: nginx/1.21.1
Content-Type: text/html
X-Frame-Options: SAMEORIGIN
Last-Modified: Fri, 18 Mar 2022 03:16:55 GMT
ETag: "1f52-5da75942e9556"
Cache-Control: max-age=60
Expires: Fri, 18 Mar 2022 03:19:04 GMT
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self' curl.haxx.se www.curl.se curl.se www.fastly-insights.com fastly-insights.com; style-src 'unsafe-inline' 'self' curl.haxx.se www.curl.se curl.se
Strict-Transport-Security: max-age=31536000
Via: 1.1 varnish, 1.1 varnish
Accept-Ranges: bytes
Date: Fri, 18 Mar 2022 03:56:07 GMT
Age: 0
X-Served-By: cache-bma1681-BMA, cache-maa10250-MAA
X-Cache: HIT, MISS
X-Cache-Hits: 1, 0
X-Timer: S1647575765.367621,VS0,VE2518
Vary: Accept-Encoding
```
Let us connect to a nonexistent page now.

```
curl -I  https://curl.se/das
HTTP/1.1 404 Not Found
Connection: keep-alive
Content-Length: 5976
Removed everything else
```
Req1 Line1:  HTTP/1.1 200 OK: specifying protocol version, 200 is the return code and OK.
Req 2 Line1: HTTP/1.1 404 Not Found:  specifying protocol version, 404 is the return code and page is not found.

Let us discuss the return code and the meaning of the return code as documented in RFC.

![HTTP error_codes](./http_codes.png?raw=true)


Line 2: `Connection: keep-alive` persistent connection, is an instruction that allows a single TCP connection to remain open for multiple HTTP requests/responses.

Line3: Content-Length: 

```
When a message does not have a Transfer-Encoding header field, a
   Content-Length header field can provide the anticipated size, as a
   decimal number of octets, for a potential payload body.  For messages
   that do include a payload body, the Content-Length field-value
   provides the framing information necessary for determining where the
   body (and message) ends.  For messages that do not include a payload
   body, the Content-Length indicates the size of the selected
   representation (Section 3 of [RFC7231]).

     Content-Length = 1*DIGIT

   An example is

     Content-Length: 3495

   A sender MUST NOT send a Content-Length header field in any message
   that contains a Transfer-Encoding header field.
```
Line 4: Server 

```
The "Server" header field contains information about the software
   used by the origin server to handle the request, which is often used
   by clients to help identify the scope of reported interoperability
   problems, to work around or tailor requests to avoid particular
   server limitations, and for analytics regarding server or operating
   system use.  An origin server MAY generate a Server field in its
   responses.

     Server = product *( RWS ( product / comment ) )

   The Server field-value consists of one or more product identifiers,
   each followed by zero or more comments (Section 3.2 of [RFC7230]),
   which together identify the origin server software and its
   significant subproducts.  By convention, the product identifiers are
   listed in decreasing order of their significance for identifying the
   origin server software.  Each product identifier consists of a name
   and optional version, as defined in Section 5.5.3.
```
List of most popular [webservers](https://www.stackscale.com/blog/top-web-servers/)

Line 5: Content-Type

```
 The "Content-Type" header field indicates the media type of the
   associated representation: either the representation enclosed in the
   message payload or the selected representation, as determined by the
   message semantics.  The indicated media type defines both the data
   format and how that data is intended to be processed by a recipient,
   within the scope of the received message semantics, after any content
   codings indicated by Content-Encoding are decoded.

Content-Type: text/html; charset=UTF-8
Content-Type: multipart/form-data; boundary=something
Directives: There are three directives in the HTTP headers Content-type.
media type: It holds the MIME (Multipurpose Internet Mail Extensions) type of the data.
charset: It holds the character encoding standard. Charset is the encoding standard in which the data will be received by the browsers.
boundary: The boundary directive is required when there is multipart entities. Boundary is for multipart entities consisting of 70 characters from a set of characters known to be very robust through email gateways, and with no white space.
```
It is slightly difficult to read RFC, important learnings can be done from curl. Curl is a beautiful tool to use from the command line.

