## First Node js Script

Make sure NodeJs is installed on your system, to test write on your terminal

```
node -v
```


> Node.js has a built-in console module that provides a simple debugging console similar to the JavaScript console mechanism provided by web browsers. The console module exports two specific components: a Console class with methods such as console.log(), console.error() and console.warn() that can be used to write to any Node.js stream; and a global console instance configured to write to process.stdout and process.stderr.

The implementation of the console object is similar in both Node.js and web browsers, which is why you can use methods like console.log() in the same way in both environments.

...

Make a file named `index.js` and type the following code in it.
open the terminal in VS Code and type `node index.js` to run the code.


```
var a = 10;
console.log(a); //output: 10
```

You can also do some mathematical operation in the REPL.
```
var a = 10;
var b = 20;
console.log(a+b); //output: 30
```
