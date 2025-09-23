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
**📦 Install MongoDB**

```
sudo apt-get update
sudo apt-get install -y mongodb-org
```

**🛸 Start The Server**
```
sudo service mongodb start
```

**✅ Verify the Service is Running**
```
sudo systemctl status mongodb
```

**📦Install [npm](https://www.npmjs.com) - Node package manager**
```
sudo apt install -y npm
```
- Install `body-parser` package
> This package helps us process JSON files passed in requests to the server.
```
sudo npm install body-parser
```
**📁Create a folder named `Books`**
```
mkdir Books && cd Books
```
- Initialize the npm project
```
npm init
```
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

