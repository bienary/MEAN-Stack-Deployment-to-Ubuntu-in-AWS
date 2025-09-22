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
**🔐 Add Certificates**
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
**📦 Install Node.js**
```
sudo apt install -y nodejs
```
