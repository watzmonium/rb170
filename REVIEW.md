Link Layer
  Frames:
    some messages are sent before frames about what to expect
    header includes source and destination MAC, how big the payload is, and a checksum footer

    MAC addresses are 6 2-digit hex numbers: FF.FF.FF.FF.FF.FF

  Hardware:
    Hub - not commonly used now, replaced by switches
      hubs use flooding to send messages

    Switches are smarter and do not flood and use a MAC Address table 

  Addressing:
    MAC addresses aren't great for long-distance routing because they're logically random
    Also if you need to replace hardware, your address has now changed, which is bad

Internet Layer:


# How does the Internet work?

# Is TCP connectionless? Why?
  TCP is connection-oriented. A connectionless protocol involves a static listening for messages and fulfilling requests as they come in. TCP establishes a channel of communication with sockets for servers and clients to communicate and can remain open as long as both client and server agree that they can. A socket can be instantiated as a socket object and it will then listen for messages in which the client socket and server socket match (four tuple)
  Connections in this way can provide benefits like requiring rules for communicating (order, ack, retransmission)
  adds to reliability

# How do sockets on the implementation level relate to the idea of protocols being connectionless or connection-oriented? 

# What does it mean that the protocol is connection-oriented?
  the protocol waits for a connection to be established before sending data

# What are the fundamental elements of reliable protocol?
  1) in order delivery
  2) handling data loss - packets not received will be resent without receiving an ack or if it times out
  3) handling duplication - duplicate data discarded and messages must be received in order
  4) error detection - via checksum

# What is pipe-lining protocols? What are the benefits of it?
  pipelining is sending multiple messages at once and retransmitting them as necessary. the number can be changed via window size

# What are the advantages and disadvantages of a Three-way handshake? 
  advantages are reliability and connection-oriented protocol
  disadvantages are speed - entire round trip of latency before messages are sent (data sent with 2nd ack)


# How does TCP facilitate efficient data transfer?
  TCP is reliable because of its properties

# What is flow control? How does it work and why do we need it?
  flow control is how each side can limit the amount of data being sent to it. Data is stored in a buffer usually as it comes to server or client and if either is overwhelmed, it can change the window size 

# How TCP prevents from receiver's buffer to get overloaded with data?
  flow control

# How do transport layer protocols enable communication between processes?

# Compare UDP and TCP. What are similarities, what are differences? What are pros and cons of using each one?
  TCP:
    A:
      reliable
      secure
    D:
      Wait for handshake
      Head of Line blocking
  UDP:
    A:
      fast
    D:
      no in-order delivery
      not reliable
      not guranteed delivery
      no congestion avoidalnce
      stateless
# What does it mean that network reliability is engineered?

# What are the obligatory components of HTTP requests? 
  METHOD and path are required, as of 1.0, also the HTTP/vernum
    headers and body are optional
  responses:
    just status message
      headers and bodies are optional

# What services does HTTP provide and what are the particular problems each of them aims to address?
  HTTP provides the ability to reqest resources and provide those resources
  it provides the ability to store state in url parameters
  pipelining as of 1.1

# What is TLS Handshake?
right after TCP ack!
  client hello - max level TLS protocol, ciphersuites
  server hello, certificate, serverhellodone - sets protocol and ciphersuits
  clientkeyexchange
  changecipherspec - start encrypted communication
  finished
  changecipherspec - start encrypted communication
  finished
# What is symmetric key encryption? What is it used for?
  not CS!
# What is asymmetric key encryption? What is it used for?
  
# Describe SSL/TLS encryption process.

# What are optimizations that developers can do in order to improve performance and minimize latency?  