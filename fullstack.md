### FULLSTACK NOTES 

#### Core of Networking: 

##### OSI Model: 
- Physical Layer (Switches, cables and bistreams)
- Datalink Layer (within same network)
- Network Layer (between different newtorks, Packets, routing, IP)
- Transport Layer (TCP, UDP, etc..)
- Session Layer (opening/closing connections)
- Presentation Layer (Compression, Encryption, etc..)
- Application Layer (FTP, telnet, etc..)

##### Physical Layer: 
- Ethernet Cables, Optic Fiber Cables, WiFi 
- data is sent as a stream of bits (0s and 1s)
    - in copper it's voltage (0V vs 5V) 
    - in OFCs it's light (on or off)
    - in radiation it's done in phase shifts of the wave 
- we need data AND clock signal, so that sender and reciever are synchronized and read the same info 
- but can we combine both of them? - manchester coding - we consider transitions from 0V to 5V as 1 and vice versa. 
    - now even if the clock rate is slightly off between sender and reciever, it would be drastically different from transitions 
    - more than perfectly synchronized clocks, manchester coding works because of transitions happening on regular intervals 

##### Datalink Layer:
- How does the reciever know where to draw the byte boundaries? 
    - Frames are defined of certain size, the data is sent in frames of fixed size. (64bytes - 1500bytes) 
    - HDLC: High Level Datalink Control (Used by ISPs, etc..)
        - A flag is defined: 01111110, The very next bit after this flag is the start of a byte of data 
        - if there are 6 consecutive 1s in the data, just add a 0 inbetween to remove confusion 
        - Ex: PPP (Point to Point Protocol) - a type of HDLC 
            - | Flag | Address | Control | Protocol | Payload | Frame Check Sequence | Flag |
            - Flag here is same as above
            - Address is not really used, because this is a point-to-point network, no other place to go 
    - Ethernet: 
        - Ethernet frame: 
            - | Preamble | Destination MAC Address | Source MAC Address | Ether Type | Payload | Frame Check Sequence |
        - For each frame of data: 
            - 96 bit-times of silence. 
            - preamble - 56 bits of alternating 1s and 0s (for the reciever to synchronize this clocks)
            - when the 101010 changes to 11, the next bit is the start of the actual data.
- Types of networks:
    - point-to-point: usually fiber optic, from city to city, heavy duty connections 
    - multipoint / broadcast (includes ethernet) 
- MAC Address: 
    - Media Access Control Address
    - Built-in address for each ethernet interface hardware 

##### Network Layer: 
- MAC addresses are visible only within an ethernet network, PPP doesn't care about MAC addresses
- How to connect two different ethernet networks? - IP Addresses!
- Routing Tables - Tables that are present in the routing servers (maybe ISPs) that tell packets where to go to reach the destination IP address
- The IP information is wrapped by the Datalink frame (ethernet/PPP/etc..) 
- How to find the router of your local network? 
    - ARP - Address Resolution Protocol 
        - | HW Addr Type | Protocol Addr Type | HW Addr Length | Protol Addr Len | OP Code | HW Addr Sender | Protocol Addr Sender | HW Addr Target | Protocol Addr Target | 
        - OP Code - is 1 or 2, 1 - who owns this IP? 2 - I own it 
        - When sending this payload, the ethernet (datalink) frame will have ff:ff:ff:ff:ff:ff as the destination address (i.e. send it to everyone on the network)
- The Destination MAC address is not known beforehand for a far away resource
    - first we use ARP to find the router, router tells it's MAC address to us, 
    - we send the data that we need to send to the far away resource with this MAC address in the ethernet frame. 
    - gets fowarded through the interwebs and finally the destination ROUTER catches the frame, and uses ARP again to check the MAC address of the destination IP 
    - then the destination router sends it to the destination resource.

##### Transport Layer: 
- IP packet has to fit inside a datalink frame (max 1500bytes), if we lose data, how to make data stream reliable? - Transport Layer Solutions! (TCP, UDP etc..)
- Problems involved: Packet loss, Reordering, Mutiple Conversations, Flow Control 
- TCP: !FIXME (clean this up)
    - "Ports" - Uniquely identifies connections, because two IPs can have multiple connections in parallel (mail, http, etc..)
    - checksum - as the other layers and encapsulating IP and datalink wrappers
    - sequence numbers - to order the data and figure it lost packets 
    - Protocol: 
        - client -SYN-> server 
        - client <-ACK-SYN- server 
        - client -ACK-> server | connection has been established  
        - sequence numbers are then used to keep track of size of data shared as well. so SEQ(N+1) = SEQ(N) + size_of_curr_data 
        - data transfer happens and N bytes of data are sent 
        - client <-FIN- server (SEQ(N))
        - client -ACK-> server (ACK(N+1))
        - client -FIN-> server (SEQ(1))
        - client <-ACK- server (SEQ(2)) | connection has been closed 

##### Internet Sockets 
- Everything in linux is a file, the networking interface is also through a file descriptor 
- socket descriptor, we have receive() and send() (more flexible) instead of read() and write()
- mainly two types of sockets: Datagram - SOCK_DGRAM and Stream - SOCK_STREAM 
- Datagram - Unreliable one way push of a message, Stream - reliable two way communication 
- Addresses: 
    - IP address - 4 byte to address a connected node on the internet
    - Port - to address an app within a node
- Order of byte: 
    - Little Endian vs Big Endian (Network Byte Order)
    - data within a byte remains same, but the order differs
    - there are performance differences, ex: casting int to short is faster with little endian 
    - communicating devices must no in what order the other reads data
    - systems call are there to take care of this (htons, htonl, ntohs, ntohl)
    - TCP/IP uses big endian while transporting packets 

#### MERN Fullstack Development 
- MongoDB - as the database application for the app 
- Express - as the HTTP server 
- React - as front end library for user experience 
- Node.js - as the JavaScript runtime environment for backend server 

##### JavaScript 
- Single threaded runtime, single call stack, to handle this JS has asynchronous "callbacks" 
- where do asynchronous "callbacks" get queued/recorded? WebAPIs 
- where does WebAPI store finished jobs? callback queue 
- how is a finished job in callback queue pushed to stack? - by the event loop  
- render queue takes precedence over callback queue and is responsible for refreshing the browser screen with newly rendered stuff

##### Express  
- Launches a HTTP Server that is able to listen to and respond to HTTP requests  
- HTTP:
    - Application Layer Protocol for the distribution of data. 
    - Methods: GET (get some data), POST (add new data), DELETE (remove data), PUT (modify data)
    - query strings: parameters provided along with URL which is made available to the web server 
    - ex: example.com/over/there?name=ferret
    - we also headers and body that you can use to send data in. 
- Authentication 

##### TypeScript 
- A type strict compiler that makes sure that all the types assigned and used are consistent in the code. 
- The typescript compiler alerts the user about errors during compilation and spits out a .js file which can be executed 
- interfaces and types are two abstractions provided by TS. 
    - interfaces are a way define types for complex objects. they are able to consolidate multiple variable types into one abstraction 
    - can be extended with other interfaces too. 
    - types are similar, they cannot be extended but they are useful to union/intersect interfaces and abstract things. 

```TS
// interface example 
interface Rectangle {
    width: number, 
    height: number 
}

// shape example 
type Rectange = {
    width: number, 
    height: number
}
```

```JS 
jwt.sign(payload, secretOrPrivateKey, [options, callback])
jwt.verify(token, secretOrPublicKey, [options, callback])
```

#### TODO: 
[Low level Networking](https://www.youtube.com/playlist?list=PLIFyRwBY_4bRLmKfP1KnZA6rZbRHtxmXi)

#### Resources: 
[JS Event Loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
[Beej's Guide to Network Programming](https://beej.us/guide/bgnet/html/split/index.html)
[Byte Order (Endianness)](https://www.youtube.com/watch?v=CounrFEsOeA)
[Ben Eater's Networking Tutorials](https://www.youtube.com/playlist?list=PLowKtXNTBypH19whXTVoG3oKSuOcw_XeW)