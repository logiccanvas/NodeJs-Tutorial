## What is Nodemon Package

Nodemon is a package that automatically restarts the node application when file changes in the directory are detected.

#### Install Nodemon Package

```
npm i nodemon
```

You can use the nodemon package in the package.json file if you like.

```
{
"scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js"
}
}
```
