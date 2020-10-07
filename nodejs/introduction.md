# Node.js
Node.js is a powerful framework developed on **Chrome's V8 javascript engine** that compiles the Javascript directly into the native machine code. It is a lightweight framework used for creating server-side web applications and extends Javascript API to offer usual server-side functionalities. It is generally used for large scale application development, especially for video streaming sites, single page application and other web applications.
Node.js is a virtual machine that uses Javascript as its scripting language and runs on a V8 environment. It works on a single-threaded event loop and a non-blocking I/O which provides high rates as it can handle number of concurrent requests.

## List down the major benefits of using Node.js
- **Fast**: Node.js is built on Google Chrome V8 javascript engine which makes it library very fast in code execution.
- **Asynchronous**: Node.js based server never waits for an API to return data thus making it asynchronous.
- **Scalable**: It is highly scalable because of its event mechansism which helps the server to respond in a non-blocking way.
- **Open Source**: Node.js has an extensive open source community which has contributed in producing some excellent modules to add additional capabilities to Node.js application.
- **No Buffering**: Node.js applications simply output the data in chunks and never buffer any data.

### Node.js used
Node.js can be used to develop
- Real-time web applications
- Network applications
- Distributed Systems
- General Purpose Applications

## Event-driven Programming
Event-driven programming is a programming approach that heavily make use of events for triggering various functions. An event can be anything like a mouse click, key press, etc. When an event occurs, a call back function is executed that is already registered with the element. This approach mainly follow the publish-subscribe pattern. Because of event-driven programming Node.js is faster when compared to other technologies.

## Event loop in Node.js
An event loop is Node.js handles all the asynchronous callbacks in an application. It is one of the most important aspects of Node.js and the reason behind Node.js have non-blocking I/O. Since Node.js is an event-driven langauge, you can easily attach a listener to an event and then when the event occurs the callback will be executed by the specific listener. Whenever functiond like setTimeout, http.get, and fs.readFile are called Node.js executed the event loop and then proceeds with the further code without waiting for the output. Once the entire operation is finished, Node.js receives the output and then executes the callback function.  This is why all the callback functions are place in a queue in a loop. Once the response is received, they are executed one by one.

