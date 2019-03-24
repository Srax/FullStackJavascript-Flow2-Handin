# Period-2 Node.js, Express + JavaScript Backend Testing, NoSQL, MongoDB and Mongoose

Note: This description is too big for a single exam-question. It will be divided up into separate questions for the exam

### Why would you consider a Scripting Language as JavaScript as your Backend Platform?

Acording to StackOverflow's [Most Popular Technologies](https://insights.stackoverflow.com/survey/2018#technology-programming-scripting-and-markup-languages), for the sixth year in a row, JavaScript is the most commonly used programming language and Node.js continues to be the most commonly used techonolgy in the "Frameworks, Libraries, and Tools" category.

The reason Node.js is one of the most commonly used frameworks is due to it's growing community and the fact that its package manager "npm", is now the largest software registry on the web.
![](https://i.imgur.com/xrNlRWA.png)

Developing with a scripting language like JavaScript is fairly simple and easy to understand, it is less cluttered and resource intensive.
Javascript is also currently the only scripting language that can run natively in your internet browser, which makes me believe it is the only good language for front-end development.

The major advantage with node.js as the backend is that javascript doesn't block input and output communication with the frontend, which means javascript can run in the background at the same time as the user is using the frontend, and it can read input and outputs simultaneously.
Another benefit to using javascript as the backend is single-threaded event loops, that is responsible for abstracting input and output from external requests. Speaking plainly, this means that Node initiates the event loop at the start, processes the input, and begins the order of operations. [source](https://thinkmobiles.com/blog/why-use-nodejs/)

### Explain Pros & Cons in using Node.js + Express to implement your Backend compared to a strategy using, for example, Java/JAX-RS/Tomcat
#### pros
1. Node.js can run right out of the box, without the use of a cms like tomcat
2. Node.js works well with other javascript based servers like mongoDB (NoSQL)
3. Speed and performance, node.js is just faster, period.

#### cons
1. Due to its asynchronous nature, Node.js relies heavily on callbacks, the functions that run after each task in the queue is finished. Keeping a number of queued tasks in the background, each with its callback can end in a callback hell, which is when people try to write JavaScript in a way where execution happens visually from top to bottom.
2. Many libraries and dependencies aren't necessarily future-proof so means they may become outdated in the future.

### Node.js uses a Single Threaded Non-blocking strategy to handle asynchronous task. Explain strategies to implement a Node.js based server architecture that still could take advantage of a multi-core Server.
We use event loops to synchronize functions (async), which allows functions to 'wait' for others functions to finish before they're called. This prevents blocking the main thread so they program won't stop running.

##### Solution 1:
API’s solves the problem, for example: Node can install a fetch module that makes fetching an async operation. File readings etc. are all built into node.

##### Solution 2:
Since the release of Node.js v10.5.0 there’s a new worker_threads (clusters) module available, which we can use to make multiple threads run at the same time at different cores.



### Explain briefly how to deploy a Node/Express application including how to solve the following deployment problems: Ensure that you Node-process restarts after a (potential) exception that closed the application Ensure that you Node-process restarts after a server (Ubuntu) restart Ensure that you can take advantage of a multi-core system Ensure that you can run “many” node-applications on a single droplet on the same port (80)
There are two solutions to make sure out Node-process restarts after a potential exception/crash.
The first one is to use a Process Manager. A Process Manager manages the 'starting' of our application, which means we no longer have to start the application manually. We can configure the Process Manager to automatically restart the application if a crash occurs.

Here is a list of popular Process Managers for Express [(source)](https://expressjs.com/en/advanced/pm.html):
1. Forever: A simple command-line interface tool to ensure that a script runs continuously (forever). Forever’s simple interface makes it ideal for running smaller deployments of Node.js apps and scripts.
2. PM2: A production process manager for Node.js applications that has a built-in load balancer. PM2 enables you to keep applications alive forever, reloads them without downtime, helps you to manage application logging, monitoring, and clustering.
3. StrongLoop Process Manager (Strong-PM): A production process manager for Node.js applications with built-in load balancing, monitoring, and multi-host deployment. Includes a CLI to build, package, and deploy Node.js applications to a local or remote system.

The second solution is to use Nodemon is a utility that monitor for changes in our source, which means it can be used to restart our application or server if it crashes.

To take advantage of a multi-core system, we can use API's, Node.js' worker_threads (clusters) or event-loops as i explained above.
To ensure that we can run "many" node-applications on a single droplet at the same port, we can use NGINX as a proxy or use some sort of load-balancer.

### Explain the difference between “Debug outputs” and application logging. What’s wrong with console.log(..) statements in our backend-code.
Acording to [HackerNoon](https://hackernoon.com/please-stop-using-console-log-its-broken-b5d7d396cf15) one of the major problems with using `console.log` givesa way too much information, which can be used to hack our system. `console.log` is also 'blocking call' which means it can impact the performance of our application by 'blocking' our other processes for a short period of time when it's called.
`console.log` can not be "disabled" which makes it very difficult to remove or disabled from the code, if not done right after debugging with it... 

The `Debug Package` for node.js allows us to debug our code, print specific debug messages and it can also easily be `DISABLED`.
Here is an example of how we can Debug in Express:
```Javascript
var a = require('debug')('a');
var b = require('debug')('b');
 
function work() {
  a('doing lots of uninteresting work');
  setTimeout(work, Math.random() * 1000);
}
 
work();
 
function workb() {
  b('doing some work');
  setTimeout(workb, Math.random() * 2000);
}
 
workb();
```
[source](https://www.npmjs.com/package/debug)

The debug's can easily be enabled or disabled by changing the `DEBUG` enviorement variable.
* if `DEBUG=*`, all our debug statements will be printed in the console.
* if `DEBUG=a`, only the `a` variable will be printed.
* if `DEBUG=*, -a`, all our debug statements will be printed, except for `a` variable.
* if `DEBUG=a,b`, is used to define multiple debugs, in this example, both the `a` variable and the `b` variable will be printed.

We can also specify what debug we want to run based on the debug's name (regex syntax):
```Javascript
var a = require('debug')('name:a');
var b = require('debug')('name:b');
```
* if `DEBUG=name:a`, the `a` variable will be debugged.
* if `DEBUG=name:a, name:b`, both the `a` variable and the `b` variable will be debugged.
* if `DEBUG=name:*` all debug statements with `name` in it will be debugged.


### Explain, using relevant examples, concepts related to testing a REST-API using Node/JavaScript + relevant packages
We can test our REST endpoints with [mocca](https://www.npmjs.com/package/mocca) and [chai](https://www.npmjs.com/package/chai).
We use `mocca` to run our tests and `chai` to make our tests more readable
Here is an example:
```Javascript
const expect = require("chai").expect;
const calculate = require("../calculator");
const fetch = require("node-fetch");
const PORT = 7890;
const URL = `http://localhost:${PORT}/api/calc/`;
let server;

describe("->-> TESTING CALCULATOR API <-<-", function() {
  //Testing the calculate.add (+) function
  it("-> 5 + 5 should be equal to 10 <-", function() {
    const result = calculate.add(5, 5);
    expect(result).to.be.equal(10);
  });

  //Testing the calculate.subtract (-) function
  it("-> 10 - 5 should equal to 5 <-", function() {
    const result = calculate.subtract(10, 5);
    expect(result).to.be.equal(5);
  });

  //Testing the calculate.multiply (*) function
  it("-> 4 * 20 should be equal to 80 <-", function() {
    const result = calculate.muliply(4, 20);
    expect(result).to.be.equal(80);
  });
  //Testing the calculate.subtract (÷) function
  it("-> 16 / 2 should be equal to 8 <-", function() {
    const result = calculate.divide(16, 2);
    expect(result).to.be.equal(8);
  });

  //Testing the calculate.subtract (÷) function where we divide by zero
  it("-> 100 / 0 should throw an error: 'Error! Cannot divide by zero' <-", function() {
    expect(() => calculate.divide(100, 0)).to.throw(
      "Error! Cannot divide by zero"
    );
  });
});

describe("\n->-> Testing the calculator API <-<-", function() {
  before(function(done) {
    calculate.calculatorServer(PORT, function(theServer) {
      server = theServer;
      done();
    });
  });
  it("-> 5 + 5 should be equal to 10 <-", async function() {
    const result = await fetch(URL + "5/add/5").then(res => res.text());
    expect(result).to.be.equal("10");
  });
  after(function() {
    server.close();
  });
});
```

