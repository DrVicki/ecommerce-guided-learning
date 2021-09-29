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

![Middleware](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/middleware.png)

We are almost done with our server for now. Create a `/signup` route to deliver the signup page.


```
//signup route
app.get('/signup', (req, res) => {
    res.sendFile(path.join(staticPath, "signup.html"));
})
```

> Add all the routes before the `404` route.

## Sign Up Page

Open your `signup.html` file. Start with a HTML5 template. Give a suitable title and link the `form.css` file to it.

First; make a loader for the page.


```
<img src="img/loader.gif" class="loader" alt="">
```

> `form.css`

```
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body{
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #f5f5f5;
    font-family: 'roboto', sans-serif;
}

.loader{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100px;
}
```

> OUTPUT
![Loader](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/loader.png)

Now we will make the form.

```
<div class="container">
    <img src="img/dark-logo.png" class="logo" alt="">
    <div>
        <input type="text" autocomplete="off" id="name" placeholder="name">
        <input type="email" autocomplete="off" id="email" placeholder="email">
        <input type="password" autocomplete="off" id="password" placeholder="password">
        <input type="text" autocomplete="off" id="number" placeholder="number">
        <input type="checkbox" checked class="checkbox" id="terms-and-cond">
        <label for="terms-and-cond">agree to our <a href="">terms and conditions</a></label>
        <br>
        <input type="checkbox" class="checkbox" id="notification">
        <label for="notification">recieve upcoming offers and events mails</a></label>
        <button class="submit-btn">create account</button>
    </div>
    <a href="/login" class="link">already have an account? Log in here</a>
</div>
```

In the code above, we use `div` for forms instead of the `form` tag. With the HTML `form` you can send a `POST` request to server but can't catch the response. We want to catch the response from the server to validate the success.

> `Form.css`

```
.logo{
    height: 80px;
    display: block;
    margin: 0 auto 50px;
}

input[type="text"],
input[type="password"],
input[type="email"],
textarea{
    display: block;
    width: 300px;
    height: 40px;
    padding: 20px;
    border-radius: 5px;
    background: #fff;
    border: none;
    outline: none;
    margin: 20px 0;
    text-transform: capitalize;
    color: #383838;
    font-size: 14px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.01);
    font-family: 'roboto', sans-serif;
}

::placeholder{
    color: #383838;
}

.submit-btn{
    width: 300px;
    height: 40px;
    text-align: center;
    line-height: 40px;
    background: #383838;
    color: #fff;
    border-radius: 2px;
    text-transform: capitalize;
    border: none;
    cursor: pointer;
    display: block;
    margin: 30px 0;
}

/* checkbox styles */

.checkbox{
    -webkit-appearance: none;
    position: relative;
    width: 15px;
    height: 15px;
    border-radius: 2px;
    background: #fff;
    border: 1px solid #383838;
    cursor: pointer;
}

.checkbox:checked{
    background: #383838;
}

.checkbox::after{
    content: '';
    position: absolute;
    top: 60%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 80%;
    height: 100%;
    pointer-events: none;
    background-image: url(../img/check.png);
    background-size: contain;
    background-repeat: no-repeat;
    display: none;
}

.checkbox:checked::after{
    display: block;
}

label{
    text-transform: capitalize;
    display: inline-block;
    margin-bottom: 10px;
    font-size: 14px;
    color: #383838;
}

label a{
    color: #383838;
}

.link{
    color: #383838;
    text-transform: capitalize;
    text-align: center;
    display: block;
}
```

> OUTPUT
![Clothing13](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/Clothing13.png)

Now we will make an alert box.

```
<div class="alert-box">
    <img src="img/error.png" class="alert-img" alt="">
    <p class="alert-msg">Error message</p>
</div>
/* alert */
.alert-box{
    width: 300px;
    min-height: 150px;
    background: #fff;
    border-radius: 10px;
    box-shadow: 0 5px 100px rgba(0, 0, 0, 0.05);
    position: absolute;
    top: 60%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 20px;
    opacity: 0;
    pointer-events: none;
    transition: 1s;
}

.alert-box.show{
    opacity: 1;
    pointer-events: all;
    top: 50%;
}

.alert-img{
    display: block;
    margin: 10px auto 20px;
    height: 60px;
}

.alert-msg{
    color: #e24c4b;
    font-size: 20px;
    text-transform: capitalize;
    text-align: center;
    line-height: 30px;
    font-weight: 500;
}
```

> OUTPUT


