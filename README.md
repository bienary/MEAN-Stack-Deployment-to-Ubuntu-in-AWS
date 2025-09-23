# 🌳**MEAN-Stack-Deployment-to-Ubuntu-in-AWS**
## 🕸️**DevOps/Cloud Engineering ~ MEAN Stack Deployment to Ubuntu in AWS**

---

# 🧱 MEAN Stack Overview

---

## 🗃️ MongoDB  
**Document Database**

- Stores and retrieves data in a flexible, JSON-like format  
- Ideal for managing large volumes of unstructured or semi-structured data

---

## 🚀 Express.js  
**Back-End Web Framework for Node.js**

- Handles HTTP requests and routing  
- Communicates with MongoDB for all Read/Write operations

---

## 🌐 Angular  
**Front-End Application Framework**

- Builds dynamic, single-page applications (SPAs)  
- Handles client-side rendering and communicates with the backend via APIs

---

## 🟢 Node.js  
**JavaScript Runtime Environment**

- Executes server-side JavaScript code  
- Manages requests and responses between client and server
---

#### ❄️ Step 0 - Preparing prerequisites

> In order to complete this project, you will need an **AWS account** and a **virtual server with Ubuntu Server OS**.

> In previous projects, we used different tools to connect to an EC2 instance. However, if you don’t want to install or launch anything outside of AWS, you can open your CLI straight from the AWS Web Console, like this:

**Task**
> You are going to implement a simple Book Register web form using MEAN Stack.
---

#### 🏗️ Step 1: Install Node.js

- `Node.js` is a JavaScript runtime built on Chrome's V8 JavaScript engine.  
- `Node.js` will be used in this tutorial to set up the **Express routes** and **AngularJS controllers**.

---

**🔄 Update Ubuntu**

```
sudo apt update
```
**⬆️ Upgrade Ubuntu**
```
sudo apt upgrade
```

<img width="1249" height="532" alt="image" src="https://github.com/user-attachments/assets/b4ffbade-b287-41f7-8d02-962ea2518f7a" />

**🔐 Add Certificates**
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

<img width="1321" height="726" alt="image" src="https://github.com/user-attachments/assets/71907adf-1614-4679-a58b-e6e303c3a525" />

**📦 Install Node.js**
```
sudo apt install -y nodejs
```

<img width="1321" height="726" alt="image" src="https://github.com/user-attachments/assets/3af80270-2a60-4ddc-b375-ace885525078" />
---

#### 🏗️ Step 2: Install MongoDB

- MongoDB stores data in flexible, `JSON-like` documents.  


**📥 Import MongoDB GPG Key**

```
sudo apt-get install -y gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
  --dearmor
```
**➕ Add MongoDB Repository**

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | \
  sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

<img width="1323" height="410" alt="image" src="https://github.com/user-attachments/assets/1751672d-02e5-4824-b97b-fa60ed682f0d" />

**📦 Install MongoDB**

```
sudo apt-get update
sudo apt-get install -y mongodb-org
```

<img width="1323" height="410" alt="image" src="https://github.com/user-attachments/assets/3e8c27ac-de63-4475-94fe-02691ff9a0a4" />

**🛸 Start The Server**
```
sudo systemctl start mongod
```

**✅ Verify the Service is Running**
```
sudo systemctl status mongod
```

<img width="1323" height="410" alt="image" src="https://github.com/user-attachments/assets/c5a9ae64-93a3-4c95-8229-0919e5a0a8a7" />

**📦Install [npm](https://www.npmjs.com) - Node package manager**
```
sudo apt install -y npm
```
- Install `body-parser` package
> This package helps us process JSON files passed in requests to the server.
```
sudo npm install body-parser
```
<img width="1323" height="279" alt="image" src="https://github.com/user-attachments/assets/f68ded05-2398-40ba-a39e-b437677f4833" />

**📁Create a folder named `Books`**
```
mkdir Books && cd Books
```
- Initialize the npm project
```
npm init
```

<img width="1321" height="726" alt="image" src="https://github.com/user-attachments/assets/1b7a0485-6741-4545-be13-f1e0f74ec1eb" />

- Add a file named `server.js`
```
vi server.js
```
- Write the following server code into `server.js`:
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3300;

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/test', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.error('MongoDB connection error:', err));

app.use(express.static(path.join(__dirname, 'public')));
app.use(bodyParser.json());

require('./apps/routes')(app);

app.listen(PORT, () => {
  console.log(`Server up: http://localhost:${PORT}`);
});
```
---

**🏗️ Step 3: Install Express and set up routes to the server**

- Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications.
- We will use `Mongoose` package which provides a straight-forward, schema-based solution to model your application data.

```
sudo npm install express mongoose
```
- Create a folder named `apps` in `Books` folder
```
mkdir apps && cd apps
```
- Create a file named `routes.js`
```
vi routes.js
```
- Write the code below into `routes.js`
```
const Book = require('./models/book');
const path = require('path');

module.exports = function(app) {
  app.get('/book', async (req, res) => {
    try {
      const books = await Book.find();
      res.json(books);
    } catch (err) {
      res.status(500).json({ message: 'Error fetching books', error: err.message });
    }
  });

  app.post('/book', async (req, res) => {
    try {
      const book = new Book({
        name: req.body.name,
        isbn: req.body.isbn,
        author: req.body.author,
        pages: req.body.pages
      });
      const savedBook = await book.save();
      res.status(201).json({
        message: 'Successfully added book',
        book: savedBook
      });
    } catch (err) {
      res.status(400).json({ message: 'Error adding book', error: err.message });
    }
  });

  app.delete('/book/:isbn', async (req, res) => {
    try {
      const result = await Book.findOneAndDelete({ isbn: req.params.isbn });
      if (!result) {
        return res.status(404).json({ message: 'Book not found' });
      }
      res.json({
        message: 'Successfully deleted the book',
        book: result
      });
    } catch (err) {
      res.status(500).json({ message: 'Error deleting book', error: err.message });
    }
  });

  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};
```

- Create a folder named `models` in the `apps` folder
```
mkdir models && cd models
```
- Create a file named `book.js`
```
vi book.js
```
- Write the code below into `book.js`
```
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  name: { type: String, required: true },
  isbn: { type: String, required: true, unique: true, index: true },
  author: { type: String, required: true },
  pages: { type: Number, required: true, min: 1 }
}, {
  timestamps: true
});

module.exports = mongoose.model('Book', bookSchema);
```

---
