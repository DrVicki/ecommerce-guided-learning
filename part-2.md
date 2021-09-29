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
![Clothing14](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/Clothing14.png)

> When the `alert-box` has a `show` class.

Great! We are finished with the signup page. Now let's make it functional. Add `form.js` to the `signup.html` page.

```
<script src="js/form.js"></script>
```

## Form.js

Select all the elements we need.

```
const loader = document.querySelector('.loader');

// select inputs 
const submitBtn = document.querySelector('.submit-btn');
const name = document.querySelector('#name');
const email = document.querySelector('#email');
const password = document.querySelector('#password');
const number = document.querySelector('#number');
const tac = document.querySelector('#terms-and-cond');
const notification = document.querySelector('#notification');
```

After selecting all the elements,add a `click` event to `submitBtn` and inside that validate form using `if else`.


```
submitBtn.addEventListener('click', () => {
        if(name.value.length < 3){
            showAlert('name must be 3 letters long');
        } else if(!email.value.length){
            showAlert('enter your email');
        } else if(password.value.length < 8){
            showAlert('password should be 8 letters long');
        } else if(!number.value.length){
            showAlert('enter your phone number');
        } else if(!Number(number.value) || number.value.length < 10){
            showAlert('invalid number, please enter valid one');
        } else if(!tac.checked){
            showAlert('you must agree to our terms and conditions');
        } else{
            // submit form
        }
})
```

In the above code, we use the `if else` which means; if this is true run the following code, and if this is not true then run the else code.

Let's understand the name validation.


```
if(name.value.length < 3){
    showAlert('name must be 3 letters long');
}
```

`if` is checking for the condition, which is written inside the `( condition )`.

`name` is our `name` element which we declared on the top the file.

`value` - since `name` is an input field it must have a value. It can be empty. So `name.value` is just returning the value of the input field.

`length` is used to count how many letters are inside a string or how many elements are inside an array. By using `name.value.length` we are checking for name's value length which is a whole number.

Once we get the length, which is a number, check for whether it is less than 3 or not.

If the condition is true, then JS will run the code written inside the `if` block, which is

```
showAlert('name must be 3 letters long');
```

That's also how we are doing other field validation.

We have to create `showAlert(msg)` function now.


```
// alert function
const showAlert = (msg) => {
    let alertBox = document.querySelector('.alert-box');
    let alertMsg = document.querySelector('.alert-msg');
    alertMsg.innerHTML = msg;
    alertBox.classList.add('show');
    setTimeout(() => {
        alertBox.classList.remove('show');
    }, 3000);
}
```

Inside the above function, we select the alert box related elements. After that, we setg up the `msg` parameter as a `innerHTML` of `alertMsg`, which is the `p` element of `alert-box`. Then we add the `show` class to `alertBox`. We use `setTimeout` to remove the `show` class after 3000 ms or 3 sec.

We are fiished with signup validation. To submit the form, make another function which will take `path` and `data` as an argument. We make a separate function because we can then use the function for both the `signup` page and `login` page.

```
// send data function
const sendData = (path, data) => {
    fetch(path, {
        method: 'post',
        headers: new Headers({'Content-Type': 'application/json'}),
        body: JSON.stringify(data)
    }).then((res) => res.json())
    .then(response => {
        processData(response);
    })
}
```

In the above code, we use a simple `fetch` method to make a `request`.  We'll make the `processData` function later.

Send the form data to the backend now.


```
else{
    // submit form
    loader.style.display = 'block';
    sendData('/signup', {
        name: name.value,
        email: email.value,
        password: password.value,
        number: number.value,
        tac: tac.checked,
        notification: notification.checked,
        seller: false
    })
}
```

> This is after form validations

Make a `signup` route inside `server.js` to handle form submission.

## Sign Up - POST

Before making the route, add this line at the top. This will enable form sharing. Otherwise; you'll not able to receive form data.

```
app.use(express.json());
```

```
app.post('/signup', (req, res) => {
    let { name, email, password, number, tac, notification } = req.body;

    // form validations
    if(name.length < 3){
        return res.json({'alert': 'name must be 3 letters long'});
    } else if(!email.length){
        return res.json({'alert': 'enter your email'});
    } else if(password.length < 8){
        return res.json({'alert': 'password should be 8 letters long'});
    } else if(!number.length){
        return res.json({'alert': 'enter your phone number'});
    } else if(!Number(number) || number.length < 10){
        return res.json({'alert': 'invalid number, please enter valid one'});
    } else if(!tac){
        return res.json({'alert': 'you must agree to our terms and conditions'});
    }       
})
```

First we extract the `data` from the `request`. Because we are sending form data from the front end, we use the same name in the backend also.

```
let { name, email, password, number, tac, notification } = req.body;
```

After that, we perform form validation. 
> Of course we did it in the front end, but it is good to have validation on the back end also, because the front end can be easily bypassed.


```
if(name.length < 3){
    return res.json({'alert': 'name must be 3 letters long'});
} else if .....
```

Notice we aren't using `value` here, because the `name` here here is not input. It's a string from the front end. In response we send `JSON` data, which looks like this.


```
JSON = {
   'key': 'value'
}
```

It is similar to JS objects, but it is used to transfer data across the web.

Great! Now we handle the `JSON` data in the front end.


```
const processData = (data) => {
    loader.style.display = null;
    if(data.alert){
        showAlert(data.alert);
    }
}
```

Hide the `loader` first. Check whether the received data contains an `alert` key or not. If it contains an `alert` key, use the `showAlert` function to alert the user. Isn't it simple.

Ok; so now let's store the user in a database or firestore.

## Storing user in firestore

Before writing more code, make sure you make a firebase project, and download the secret key file from the dashboard. [You can refer to Firebase instructions here to download the key.](https://firebase.google.com/docs/admin/setup#:~:text=Create%20a-,Firebase,-project)

Once you have the key file, put it into your project folder outside the `public` folder.

Then `init` the firebase inside `server.js`.
// firebase admin setup


```
let serviceAccount = require("path of key file");

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount)
});

let db = admin.firestore();
```

Inside the `signup` POST route, store the user in database after validations.


```
// store user in db
db.collection('users').doc(email).get()
.then(user => {
    if(user.exists){
        return res.json({'alert': 'email already exists'});
    } else{
        // encrypt the password before storing it.
        bcrypt.genSalt(10, (err, salt) => {
            bcrypt.hash(password, salt, (err, hash) => {
                req.body.password = hash;
                db.collection('users').doc(email).set(req.body)
                .then(data => {
                    res.json({
                        name: req.body.name,
                        email: req.body.email,
                        seller: req.body.seller,
                    })
                })
            })
        })
    }
})
```

In firebase we have collections, which store groups of data. In this case we have a `user`s collection in our firstore. `db.collection` is used to access the collection. When you are in collection, you can get the document by calling `doc(docname)` and after you found the doc, you can get it by calling the `get()` method. Then you can access it using `then`. 


```
db.collection('users').doc(email).get()
.then(...)
```

In the above code, we are checking if the email is already exists in our database or not. If it is we send an alert. And if not, we store the user in the database.


```
if(user.exists){
    return res.json({'alert': 'email already exists'});
} else{
    // encrypt the password before storing it.
    bcrypt.genSalt(10, (err, salt) => {
        bcrypt.hash(password, salt, (err, hash) => {
            req.body.password = hash;
            db.collection('users').doc(email).set(req.body)
            .then(data => {
                res.json({
                    name: req.body.name,
                    email: req.body.email,
                    seller: req.body.seller,
                })
            })
        })
    })
}
```

`bycrypt` is the encrypt package. But to hash the password, you can just code it. `genSalt` is how much salting you want to perform on a text. `hash` is to covert the text into hash. Everything else is the same till `doc()`. This time we don't have to `get()` we have to `set()` which is self explanatory. In response, we send the users `name`, `email`, and `seller` status to the  front end.

Let's store it in front end.


```
const processData = (data) => {
    loader.style.display = null;
    if(data.alert){
        showAlert(data.alert);
    } else if(data.name){
        // create authToken
        data.authToken = generateToken(data.email);
        sessionStorage.user = JSON.stringify(data);
        location.replace('/');
    }
}
```

Use session storage to store the user data inside `session`. We can't simply use users email to validate its authenticity. We need something we can validate. Generate an auth token for the user. 

First add the `token.js` file to `signup.html.

```
<script src="js/token.js"></script>
```

Create `generateToken` function.

> `Token.js`

```
let char = `123abcde.fmnopqlABCDE@FJKLMNOPQRSTUVWXYZ456789stuvwxyz0!#$%&ijkrgh'*+-/=?^_${'`'}{|}~`;

const generateToken = (key) => {
    let token = '';
    for(let i = 0; i < key.length; i++){
        let index = char.indexOf(key[i]) || char.length / 2;
        let randomIndex = Math.floor(Math.random() * index);
        token += char[randomIndex] + char[index - randomIndex];
    }
    return token;
}
```

In the above code, we generate a text witha  2 letters index number to give the original text an index from the char string. 

Now we want a function to validate the token.

```
const compareToken = (token, key) => {
    let string = '';
    for(let i = 0; i < token.length; i=i+2){
        let index1 = char.indexOf(token[i]);
        let index2 = char.indexOf(token[i+1]);
        string += char[index1 + index2];
    }
    if(string === key){
        return true;
    }
    return false;
}
```

Great! We are almost finished with the page. Before now, we have successfully stored the `user` in session. Let's validate it.

> `form.js`

```
// redirect to home page if user logged in
window.onload = () => {
    if(sessionStorage.user){
        user = JSON.parse(sessionStorage.user);
        if(compareToken(user.authToken, user.email)){
            location.replace('/');
        }
    }
}
```

We added the load event to the window, which is checking if the `user` is in session or not. If `user` is in session, we validate the auth token. If it is legitimate, we redirect the `user` to the home page, because he/she rdoesn't need to sign up again.


## Summary

Great! Our sign up page is finished. Since the blog is being too much lengthy. I think that enough for today. But yes, 

In the second part, we made a login page and seller's dashboard. l




