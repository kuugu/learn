### FULLSTACK NOTES 

##### Core of Networking: 


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
    - query strings: parameters provided along with URL which is made available to the web server (ex: example.com/over/there?name=ferret)
- Authentication 

##### TODO: 
[Low level Networking](https://www.youtube.com/playlist?list=PLIFyRwBY_4bRLmKfP1KnZA6rZbRHtxmXi)

#### Resources: 
[JS Event Loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)