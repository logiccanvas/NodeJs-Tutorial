## What is Express.js

Express.js is a minimal and flexible Node.js web application framework that provides a robust set of features to develop web and mobile applications. It simplifies the process of building scalable and maintainable server-side applications.

**Installation**
```
npm install express
```

**Creating a Basic Express App**
```
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});

```

**Creating Routes**
```
// Import express
const express = require('express');
const app = express();

// Define routes
app.get('/', (req, res) => {
  res.send('Home Page');
});

app.get('/about', (req, res) => {
  res.send('About Page');
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});

```



**CRUD API Example**
```
const express = require('express');
const app = express();
const port = 3000;

// Sample data
const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' },
];

// Middleware to parse JSON
app.use(express.json());

// Routes
app.get('/users', (req, res) => {
  res.json(users);
});

app.get('/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const user = users.find(u => u.id === id);

  if (user) {
    res.json(user);
  } else {
    res.status(404).send('User not found');
  }
});

app.post('/users', (req, res) => {
  const newUser = req.body;
  users.push(newUser);
  res.status(201).json(newUser);
});

app.put('/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const updateUser = req.body;
  const index = users.findIndex(u => u.id === id);

  if (index !== -1) {
    users[index] = { ...users[index], ...updateUser };
    res.json(users[index]);
  } else {
    res.status(404).send('User not found');
  }
});

app.delete('/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index !== -1) {
    const deletedUser = users.splice(index, 1);
    res.json(deletedUser[0]);
  } else {
    res.status(404).send('User not found');
  }
});

// Start the server
app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

> In this CRUD API example, the server provides endpoints for listing users, getting a user by ID, creating a new user, updating a user, and deleting a user. The sample data is stored in memory for simplicity.
