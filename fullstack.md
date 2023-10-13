### FULLSTACK NOTES 

#### Core of Networking: 

##### OSI Model: 
- Application Layer (FTP, telnet, etc..)
- Presentation Layer (Compression, Encryption, etc..)
- Session Layer (opening/closing connections)
- Transport Layer (TCP, UDP)
- Network Layer (between different newtorks, Packets, routing)
- Datalink Layer (within same network)
- Physical Layer (Switches, cables and bistreams)

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
- Authentication 


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