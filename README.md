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
