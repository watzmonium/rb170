# What is the internet?

  - It's not a big truck, it's not something you just dump something on.
  - It's, it's a series of tubes. And if you don't understand how those tubes can be filled, and if they're filled
    when you put your message in, it gets in line and it's going to be delayed by anyone who puts into that tube
    enormous amounts of material
  - Enormous amounts of material.

  - Packets being sent through a distributed physical network
  - A large number of independently operated networks
    "a network of networks"

# What is a network?

  - Two devices connected to communicate and exchange data
  #LAN
    - Multiple devices connected via some type of hub or switch
    - Physical networks known as LAN (local area network)
    - Wireless hubs also known as WLAN (wireless local area network)

  In order to increase the size of the network, routers are needed.
    do modems have built-in routers?

# Protocols

  - protocols ensure all devices and systems are able to communicate
    - think of like greetings or handshakes
  - "A set of rules governing the exchange or transmission of data"
  i.e.
    IP - internet protocol
    SMTP - Simple mail transfer protocol
    TCP - Transmission control protocol
    HTTP - Hypertext transfer protocol
    Ethernet - physical protocol for standard data transmission
    FTP - file transfer protocol
    DNS - domain name service
    UDP - user datagram protocol
    TLS - transport layer security
    MAC - media access control
    PDU - protocol data unit

  #protocol anatomy
    - sntactical rules for structure
    - message transfer rules for flow and order

# Models of the internet

  #OSI
    physical | Data link | network | transport | session | presentation | application
    (bits, waves) (frame)  (packets)(packet parts)         (data)
    hub/switch| local net| between | packets      auth      data processing app
  #TCP/IP
          Link           | internet|   transport    |         Application

      frame data            IP        TCP/UDP                 data

  - Protocol data units (PDU)
    amount or block of data transferred over a network
    - different names depending on layers
      link layer is 'frame', internet/network layer is 'packet', trasport is 'segment' (TCP)
      or datagram (UDP)

    - All PDU has a header, a data payload, and sometimes a footer
      | header |    DATA                                 |footer
      metadata

    this encapsulation allows layers to not care WHAT kind of message or data is being sent, just what it needs to do with it

# physically sending data

  bandwith
    - maximum transmission capabiity
    - measured in bitrate

  can send electrical signals with a clock
    Ethernet is cheap but can't send far
  can send light
    fiberoptic cable
    angle means multiple messages sent at same time - won't interfere
    fast and little degredation
    expensive and hard to work with
  can send radio waves
    send waveforms
    short range

  #Latency
    What actually causes latency?
      1) Propagation delay
        basically distance - ratio between distance and speed
      2) Transmission delay
        traceroute delay based on number of times routed
      3) Processing delay
        issues at the routers themselves
      4) Queueing delay
        issues with backup at routers

    Also:
      last-mile latency
        - ISP issues and LAN issues
      RTT
        round trip time

# Link/data link layers

  lowest level of encapsulation

  Ethernet protocol
    - framing
      - ethernet frames are a PDU that encapsulates data from internet/network layer above

      destination MAC | source MAC | length | (DSAP|SSAP|Control)             | data    | FCS
                                                each 8 bits                               frame check sequence
                                                SAPs are network protocol                 checksum
                                                control is frame com control
      frame -> IFG -> frame -> IFG
      IFG - interframe gap - pause between frames
    - addressing
      MAC addresses
        6 hex 2-digit hex numbers

# internet / network layers

  UDP of this unit is a PACKET
    - header + (TCP seg | UDP datagram)
    - header:
      IP version
      ID/flags/fragment offset (i.e. 1/6 packets)
      TTL - time to live
      protocol - TCP/UDP
      checksum
      source/destination IPs

    IPv4
      32 bit address, 4 numbers 0-255

    splitting network into part is 'sub-netting'

    IPv6
      128 bit address, 8 16 bit blocks
      different header packet structure
      no checksum (leaves it to the link layer )

# Transport/Session || Transport Layers
  - How do transport layer protocols enable communication between processes?
    - what are multiplexing and demultiplexing?
    - How do ports and IP addresses work to provide this functionality?

  - Network reliability is engineered

  Multiplexing
    - sending multiple signals over a single channel
      i.e. spotify and email and discord, and dota all at the same time over the same channel

  Demultiplexing
    - taking a multiplexed signal and separating out the different components

  Ports:
    - mutli and demultiplexing take place through network ports
    - a port is an identifier for a specific process running on a host, 0-65535
      0-1023 are reserved for common network services
        80 is HTTP
        20 is FTP
        25 is SMTP
      1024-49151 are registered ports. They are assigned by private entities like microsoft for stuff
      49152-65535 are dynamic or private ports 

  PDUs
    - port info is included in the trasport layer PDU
    - IP is then encapsulated. IP + port allows specific application(multi/demultiplexing)
      between different applicatinos on different machines
    - The IP/Port combo is a 'communication end point' aka a #SOCKET#
    - For now think of a socket as an IP + port i.e. 192.168.1.1:8080
    
  Connectionless system
    - 1 computer listening at 1 port and processed as needed

  Connection-oriented system
    - computer still listens at port, but now it might do other things
      i.e. directly instantiates a new socket object to interact with the socket of the sender
    - 'four touple'
        source IP, source port, destination IP, destination port
    
    - this dedicated connection provides some advantages
      1) order of messages
      2) ackloweldgements
      3) retransmission

    Reliability
      apparently all this shit is unreliable?
      the frames or packets (ipv4) have checksums to see if the higher-level stuff isn't screwed up
      if it is, it's just discarded
      then it's just not replacedm making this UNRELAIBLE

      How do we build reliability?

        Idea 1 - send an acknowledgement
          sender sends, waits for OK, then sends next. repeat.
            BUT WHAT IF? the recipient never receives the message?
                      OR the acknowledgment is sent but that never gets received?

        Idea 2 - re-send if we don't get acknowledgement within a certain timeframe.
            BUT WHAT IF? The acknowledgement was sent and lost? then we get repeat messages
                      OR the acknowledgement is sent but takes too long?

        Idea 3 - add sequence numbers to the message
          same as above except now waits to send the next message of the set in order
            1st message sent, wait for acknowledgement or timeout
            if fail, resend same message, if duplicate, assumes receiver never got confirmation and resends

          This one covers fundimental reqs for reliable data transfer:
            1) in order
            2) error detection via checksum
            3) missing data resent based on confirmations and timeouts
            4) duplicate data eliminated b/c sequcence #s

            This is reliable but very inefficient
            "stop and wait"

            Use "pipelining" - send multiple messages at once
              examples: go-back-N, selective repeat
              both systems the sender impliments a 'window' repesenting max num of msg at a time

# TCP
  TCP adds the abstraction of reliable communication on an unreliable channel

  Segments are the PDU of TCP
    header includes source and destination ports which allow for multiplexing

      checksum for error handling
        - makes others a bit redundant, which is why ipv6 dropped them

      sequence and acknowledgement numbers
        sequence for in-order delivery
        acknowledgement for handling data loss and duplication

      window size for flow control

      flag fields
        - 1 bit boolean fields

  TCP is connection-oriented (active socket communication)
    it uses the 'three-way handshake'
      sender sends SYN flag
      responder sends SYN ACK
      sender responds with ACK
      CONNECTION ESTABLISHED
    
    These flags manage the `connection state`
      the states are:
        LISTEN <- important
        SYN-SENT
        SYN-RECEIVED
        ESTABLISHED < - important
        FIN-WAIT-1
        FIN-WAIT-2
        CLOSE-WAIT
        CLOSING
        LAST-ACK
        TIME-WAIT
        (CLOSED) <- fictional state (just means its over)

  Flow control prevents the sender from sending TMI
    info is stored in a buffer
    each side can let the other know how much buffer using the window header

  Congestion avoidance
    more data being sent than the network can handle
    TCP reduces transmission window size if congestion detected

  Issues with TCP
    - speed
    - HoL (head of line) blocking
      holding up other messages because order necessary

# UDP

  user datagram procotol

  PDU is a datagram

  header is basically just ports, length, and checksum
    checksum optional with ipv4

  differences from TCP
    no confirmation of delivery
    or order
    no congestion or flow mechanism
    no connection-state protocol, connectionless!

  advantages
    fast
    flexible

# Application Layer

  - This is NOT the application itself! A set of protocols that provide commication services to applications
  - Sometimes the application interacts with transport to open sockets, but mostly this layer

# HTTP and the web

  - not the same thing!
  - WWW or web for short is a SERVICE that can be accessed via the network of networks internet
    - vast information system comprised of resources
    - set of the hypetext document
  - HTTP is the means by which applications interact with web resources
    - requests and protocols like GET

# Servers

  - Web server
  - Application server
  - Data store