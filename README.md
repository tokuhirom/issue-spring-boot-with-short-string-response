# spring boot @RestController and short string response issue

When controller returns bytes shorter than 171 bytes, curl can't get the response.

    $ curl -v http://localhost:8080/l\?n=170
    *   Trying ::1...
    * TCP_NODELAY set
    * Connected to localhost (::1) port 8080 (#0)
    > GET /l?n=170 HTTP/1.1
    > Host: localhost:8080
    > User-Agent: curl/7.54.0
    > Accept: */*
    >
    < HTTP/1.1 200
    < Content-Type: text/plain;charset=UTF-8
    < Content-Length: 170
    < Date: Tue, 28 Nov 2017 07:11:38 GMT
    <
    * Connection #0 to host localhost left intact

Curl can get the bytes when the server returns 171+ bytes.

    $ curl -v http://localhost:8080/l\?n=171
    *   Trying ::1...
    * TCP_NODELAY set
    * Connected to localhost (::1) port 8080 (#0)
    > GET /l?n=171 HTTP/1.1
    > Host: localhost:8080
    > User-Agent: curl/7.54.0
    > Accept: */*
    >
    < HTTP/1.1 200
    < Content-Type: text/plain;charset=UTF-8
    < Content-Length: 171
    < Date: Tue, 28 Nov 2017 07:11:41 GMT
    <
    * Connection #0 to host localhost left intact
    ..........................................................................................................................................................................

## with telnet

### n=170 HTTP/1.0

    $ telnet localhost 8080
    Trying ::1...
    Connected to localhost.
    Escape character is '^]'.
    GET /l?n=170 HTTP/1.0
    
    HTTP/1.1 200
    Content-Type: text/plain;charset=UTF-8
    Content-Length: 170
    Date: Tue, 28 Nov 2017 07:31:29 GMT
    Connection: close
    
    ..........................................................................................................................................................................Connection closed by foreign host.

## Note

This issue is reproduced on OSX curl only. It's not reproduced on linxu curl.
