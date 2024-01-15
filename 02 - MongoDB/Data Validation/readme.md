# Mongo: Data Validation

<details>
  <summary>Basic Validation</summary>
    Validation is meaningful in mongoose (ORM) only, MongoDB doesn’t care or implement that.
  
##### We define name as required

```
const couseSchema = new mongoose.Schema({
  name: { type: String, required: true },
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
});
```

##### If we don’t pass name in the payload, it throughs error

```
const createCourse = async () => {
  const course = new Course({
    author: "Kashif Hussain",
    tags: ["Javascript", "Angular"],
    isPublished: false,
  });

  try {
    await course.save();
  } catch (err) {
    console.log(err.message);
  }
};
```

<span style="color: red">Course validation failed: name: Path name is required.</span>

...

##### We can also validate with or without saving

```
const createCourse = async () => {
  const course = new Course({
    author: "Kashif Hussain",
    tags: ["Javascript", "Angular"],
    isPublished: false,
  });

  try {
    await course.validate();
    // await course.save();
  } catch (err) {
    console.log(err.message);
  }
};
```

</details>

<details>
  <summary>Built-in Validators</summary>

##### Conditionally validate a property e.g. <code>price</code> becomes required if <code>isPublished</code> is set to true

```
const couseSchema = new mongoose.Schema({
  name: { type: String, required: true },
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
  price: {
    type: Number,
    required: function () {
      return this.isPublished;
    },
  },
});
```

Error: <span style="color: red">price: Path <code>price</code> is required., name: Path <code>name</code> is required</span>

> Note: the error message is simply a string.

...

##### To print out multiple messages for each error

```
const createCourse = async () => {
  const course = new Course({
    // name: "Node.js Basics",
    author: "Kashif Hussain",
    tags: null,
    isPublished: true,
    category: "web",
    price: 5,
  });

  try {
    await course.validate();
    // await course.save();
  } catch (err) {
    for (field in err.errors) {
      console.log(err.errors[field].message);
    }
  }
};
```

##### Following we have <code>minLength</code>, <code>maxLength</code>, <code>enum</code> and <code>match</code> validators on string and <code>min</code> <code>max</code> on number

```
const couseSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    minLength: 5,
    maxLength: 255,
    // match: /pattern/,
  },
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
  category: {
    type: String,
    required: true,
    enum: ["web", "mobile", "network"],
		// lowercase: true,
		// uppercase: true,
		// trim: true,
  },
  price: {
    type: Number,
    min: 10,
    max: 200,
    required: function () {
      return this.isPublished;
    },
    get: v => Math.round(v),
    set: v => Math.round(v),
  },
});
```
</details>


<details>
  <summary>Custom Validators</summary>

##### Tag property has custom validation and error message
```
tags: {
    type: Array,
    validate: {
      validator: function (value) {
        return value?.length > 0;
      },
      message: "A course should have at least one tag.",
    },
  }
```

##### if we send tag as empty object or comment it out
```
const course = new Course({
    name: "Node.js Basics",
    author: "Kashif Hussain",
    tags: [],
    isPublished: true,
    category: "web",
    price: 10,
  });
```
<span style="color: red">Course validation failed: tags: A course should have at least one tag.</span>

...

##### even if we send null value we get same error as above because we have already taken care of nullish value in validator e.g. <span style="color: red">return value?.length > 0;</span>
```
const course = new Course({
    name: "Node.js Basics",
    author: "Kashif Hussain",
    tags: null,
    isPublished: true,
    category: "web",
    price: 10,
  });
```

##### Custom asynchronous validator example
```
tags: {
    type: Array,
    validate: {
      isAsync: true,
      validator: function (value, callback) {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            const result = value?.length > 0;
            resolve(result);
          }, 3000);
        });
      },
      message: "A course should have at least one tag.",
    },
  }
```
</details>
