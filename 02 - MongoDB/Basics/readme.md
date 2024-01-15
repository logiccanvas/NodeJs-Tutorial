# MongoDB Basics

##### Create Basic connection with MongoDB

```
const mongoose = require("mongoose");

mongoose
  .connect("mongodb://127.0.0.1:27017/education")
  .then(() => console.log("connection established"))
  .catch((err) => console.error("cound not connect to Mongoose server", err));
```

##### Create Schema and define model to create instance of schema

```
const couseSchema = new mongoose.Schema({
  name: String,
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
});

const Course = mongoose.model("Course", couseSchema);
```

##### Create a document in mongoDB

```
const createCourse = async () => {
  const course = new Course({
    name: "Angular",
    author: "Hussain",
    tags: ["Javascript", "Frontend", "Angular"],
    isPublished: false,
  });

  await course.save();
};

createCourse();
```

##### Query get command

```
const getCourses = async () => {
  const courses = await Course.find();
  console.log("courses list: ", courses);
};

getCourses();
```

##### Query get command with filters

```
const getCourses = async () => {
  const courses = await Course.find({
    author: "Hussain",
    isPublished: true,
  })
	.limit(10)
	.sort({ name: 1, tags: 1 })
	.select({ name: 1, tags: 1, author: 1 });

	console.log("courses list: ", courses);

	/*
		1. find command without any parameters fetches all records and with para meters
		e.g. { author: '...' } returns only records with matched values
		2. select filter returns only matched key value pairs only instead of whole document
		3. limit filter returns number or records
		4. sort filter sorts records on selected key and 1 returns asc -1 returns desc
	*/

};

getCourses();
```

<details>
  <summary>Comparison Operators</summary>
  <ul>
    <li>eq (equal)</li>
    <li>ne (not equal)</li>
    <li>gt (greater than)</li>
    <li>gte (greater than or equal to)</li>
    <li>lt (less than)</li>
    <li>lte (less than or equal to)</li>
    <li>in</li>
    <li>nin (not in)</li>
  </ul>
</details>

<details>
  <summary>Logical Operators</summary>
  <ul>
    <li>or</li>
    <li>and</li>
  </ul>
  examples

```
const getCourses = async () => {
const courses = await Course.find()
		.or([ { author: "Mosh" }, { isPublished: false } ]);
};

const getCourses = async () => {
const courses = await Course.find()
		.and([ { author: "Mosh", isPublished: false } ]);
};
```

</details>

<details>
  <summary>Filter Query with Regular Expressions</summary>
  The syntax is .find({ key: /pattern/ })

Example Starts with string Kashif

```
  const getCourses = async () => {
    const courses = await Course.find({
      author: /^Kashif/,
    });
  };
```

Ends with Hussain and case insensitive

```
const getCourses = async () => {
  const courses = await Course.find({
    author: /Hussain$/i,
  });
};
```

contains Kashif at any position in the string and case insensitive

```
const getCourses = async () => {
  const courses = await Course.find({
    author: /.*Kashif.*/i,
  });
};
```

</details>

<details>
  <summary>Return only count of documents</summary>
  
```
const getCourses = async () => {
  const courses = await Course.find().count();
};
```
</details>

<details>
  <summary>Pagination</summary>


```
const getCourses = async () => {
  const pageNumber = 2;
  const pageSize = 10;

  const courses = await Course.find({
    author: "Kashif Hussain",
    isPublished: true,
  })
  .skip((pageNumber - 1) * pageSize)
  .limit(pageSize);
};
```
</details>

<details>
  <summary>Import collection</summary>
mongoimport --db mongo-excercises --collection courses --drop --file exercise-data.json --jsonArray
</details>

##### Practice

<details>
  <summary>Get courses which have tag ‘backend’, sort by name and display name and author</summary>

```
const getCoursesPriceWise = async () => {
  return await Course.find({
    isPublished: true,
    // tags: { $in: ["frontend", "backend"] },
  })
    .or([{ tags: "frontend" }, { tags: "backend" }])
    .sort("-price")
    .select("name price");
};
```
</details>


<details>
  <summary>Get courses with tags either backend or frontend, sort price descending , display only name and price</summary>

```
const getCoursesPriceWise = async () => {
  return await Course.find({
    isPublished: true,
    // tags: { $in: ["frontend", "backend"] },
  })
    .or([{ tags: "frontend" }, { tags: "backend" }])
    .sort("-price")
    .select("name price");
};
```
</details>

<details>
  <summary>Get courses which have prices 15 or greater, and includes ‘by’ in name</summary>

```
const getCoursesPriceMoreAndWordBy = async () => {
  return await Course.find({
    isPublished: true,
  }).or([
    {
      price: { $gte: 15 },
    },
    {
      name: /.*by.*/i,
    },
  ])
  .sort('-price');
};
```
</details>

<details>
  <summary>Update a document with findById</summary>

```
const updateCourse = async (id) => {
  const course = await Course.findById(id);

  if (!course) return;

  //   course.isPublished = true;
  //   course.author = "Kashif Hussain";

  course.set({
    isPublished: true,
    author: "Kashif Hussain",
  });

  return await course.save();
};
```
</details>

<details>
  <summary>Update a document with findOne</summary>

```
const updateCourse = async (name) => {
  const course = await Course.findOne({ name: name });

  if (!course) return;

  course.set({
    isPublished: true,
    author: "Kashif Sab",
  });

  return await course.save();
};
```
</details>

<details>
  <summary>Update with update first approach instead of query first approach</summary>

```
const updateCourse = async (id) => {
	// await Course.findByIdAndUpdate(id, {.....}, {new: true});
	// new: true will return updated document

  return await Course.updateOne(
    { _id: id },
    {
      $set: {
        author: "New Teacher",
        isPublished: false,
      },
    }
  );
};
```
</details>
Note: Note: look for mongodb update operators

...


<details>
  <summary>Remove a document</summary>

```
const removeCourse = async (id) => {
  //   return await Course.deleteOne({ _id: id });
  //   return await Course.deleteMany({ _id: id });
  return await Course.findByIdAndRemove(id);
};
```
</details>
