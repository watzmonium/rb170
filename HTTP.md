# HTTP BOOK NOTES #

  - clients and servers
    servers serve clients with HTTP protocol
    request-response protocol
  
  most of today is HTTP 1.1, but 2 is gaining traction and 3 is in development

  When you enter a URL

    1) Browster makes an HTTP request - sent to network interface

    2) If DNS cache has the IP address for that URL, it just directs you,
      else, a DNS request is made to get the IP

    3) once the address is set, the packaged-up HTTP request is sent to the IP required

    4) The remote server accepts the request and sends a response back to your network interface which sends to browser

    5) Browser displays web page

  Resources: 
    files of any type can be resources that you get from a server

  STATELESSNESS
    HTTP is a stateless protocol
    this means it doesnt preserve state
    it also makes the web resiliant, distribuetd and hard to control
    it also makes it so difficult to secure and build on top of

  # URLs

    example : http://www.example.com:88/home?item=book
    - http - SCHEME
      comes before colon and tells your browser HOW to access this resource
      other schemes are ftp, mailto, git

    - www.example.come - HOST
      where the resource is located

    - :88 - PORT NUMBER
      only required if you want to use an alternate port
      80 is the default for ALL HTTP requests

    - /home/ - PATH
      what local resource is being located
    
    - ?item=book - QUERY string
      made up of query parameters, used to send data to the server
      because query strings are passed through the URL, they are only used in GET requests
      they have a max length
      theyre public/visible
      space and special characters cant be used without url encoding

    URL encoding

      - URLs only accept certain ASCII characters
      - other characters have to be encoded
        %HH (h for hex value)
          i.e. space == %20 tommy%20hilfiger
               ! == %21 more%21.html
               + == %2B
               # == %23

      - characters must be encoded if they arent
        1) standard ASCII
        2) unsafe because of misinterpretation like % or < >
        3) ARE reserved like & / ? : @ 

      Safe characters:
        $-_.+!'()",

  # Making requests

    POST/GET

    STATUS CODES
      200 - OK
      302 - FOUND - (probably moved temporarily)
      403 - FORBIDDEN
      404 - NOT FOUND
      500 - INTERNAL SERVER ERROR


    browsers abstract a lot of the HTTP request/response cycle

    HTTP requests have
      HEADERS
        general
        response
        request
      BODY
        data

  # Statefullness
    - Sessions
      if the server sends a unique token to the client, this can be used to establish a session
      the token is called the `session identifier`

      some issues:
        every request needs to be examined for a session id
        THEN see if it's still valid
        THEN retrieve the data
        THEN recreate the application state

        this is a lot for the server to do to maintain statefulness

    - Cookies
      a piece of data sent by the server stored on a client in the request/response cycle
      stores session info
      the actual session data is stored on the server (prob for security and other issues)
      the client cookie gets compared to the server to identify the session

      set in response headers

    - AJAX (asynchronous javascript calls)

 AJAX
    asynchronous javascript and XML
    allows browsers to issue requests and responses WITHOUT a page refresh
    they are like normal server requests BUT the browser processes this and not the server

  # Security

    HTTPS
      encrypts data sent over the internet
      else, someone could do packet sniffing to perhaps spoof your session id and hijack your acct
      used to use SSL but now uses TLS (transport layer security)

    Same origin policy
      unrestricted access between resources who have same scheme, url, and port
      CORS (cross origin resource sharing) gets around this

    Session hijacking
      hacker gets your session id
      reset session
      expiring sessions
      HTTPS

    Cross-site Scripting (XSS)
      allowing users to enter data that will be diplayed on the site
      santize inputs
      escape all special characters

    
