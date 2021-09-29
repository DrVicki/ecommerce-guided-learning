# Full-Stack e-Commerce Store Part 2

Let's do the second part of our fullstack e-com website series. 

In this part, you'll make a node server to run your website on a localhost, then you'll learn to do form validations and storing users in firestore. In Part 2, we'll make a signup page/ login page, logout feature, and sellers dashboard.

## Code

You can see below our project's folder structure. We have some new files compared to what we had in the previous part.

![FolderStructure-2](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/folderstructure2.png)

### Let's start coding.

## NPM Init

Start with the server. 
Open the previous code folder in the terminal or with cmd prompt. 
Run `npm init`. 
This will initialize the NPM to the project. After that, install some packages by running this command.

```
npm i express.js nodemon firebase-admin bcrypt
```
`express.js` - is to create a server

`nodemon` - is to run server continuously.

`firebase-admin` - is to access firebase from backend.

`bcrypt` - is to encrypt the user's password before storing it inside our database.

Once you are done with the installation, you'll see `package.json` on your directory. Open the file and changes scripts object.

```
"scripts": {
    "start": "nodemon server.js"
}
```

This will make a start command to use with NPM. Now if you haven't created a `server.js` file; make one. 

Let's make the server.

## Server

Open `server.js` file. Start by importing the packages we just installed.


```
// importing packages
const express = require('express');
const admin = require('firebase-admin');
const bcrypt = require('bcrypt');
const path = require('path');
```

We didn't install the path package/library as it is a default package which comes with nodeJS. `path` allows access to the path or allows path-related actions.


```
// declare static path
let staticPath = path.join(__dirname, "public");
```

Make public folder's path a static path. **Static path** is a path which tells the server where it has to look for the files.


```
//intializing express.js
const app = express();

//middlewares
app.use(express.static(staticPath));

app.listen(3000, () => {
    console.log('listening on port 3000.......');
})
```

In the above code, we make an express server and listen for requests on port 3000.

Make `/` , `/404` routes.

```
//routes
//home route
app.get("/", (req, res) => {
    res.sendFile(path.join(staticPath, "index.html"));
})
```

Start your server now by running `npm start` on the terminal. 

Open `localhost:3000` on your chrome to view the page. If the server is working, you'll see the `index.html` page.

For the `404` route, we'll use middle ware. Make sure to add this middle ware at the very bottom of the server. Otherwise you'll get a `404 page even if you are on some defined route.


```
// 404 route
app.get('/404', (req, res) => {
    res.sendFile(path.join(staticPath, "404.html"));
})

app.use((req, res) => {
    res.redirect('/404');
})
```

We made a separate `404` page and redirected user on making a request to any unknown route. 

If we deliver the `404` page through middle ware, we'll definitely get the page, but if we go to the nested routes, we'll get a page without styles. See the illustration below.
