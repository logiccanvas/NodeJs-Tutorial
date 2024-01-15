## Make a Basic Server?

1. Create a file named server.js in the root directory of your project.

```
const http = require('http');
http.createServer().listen(4500);

http.createServer((req, res) => {
    res.write('Hello World');
    res.end();
}).listen(4500);
```

You can also send a html tag to the client.
```
http.createServer((req, res) => {
    res.write('<h1>Hello World</h1>');
    res.end();
}).listen(4500);
```

2. Now, run the server using the following command.
```
node server.js
```

3. Now, open the browser and type localhost:4500 in the address bar. You will see the following output.
```
Hello World
```
