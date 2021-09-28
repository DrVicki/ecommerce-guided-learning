# Full-Stack e-Commerce Store Part 1

We'll learn to create an e-commerce website using HTML, CSS, and JavaScript. This is a part of full stack e-commerce website. 
  * In this part we'll only create front end page's UI. We'll create 4 pages in this tutorial - Home page, Project Page, Search Page, and 404 page.

## Code

You can see our project's folder structure below.
![File Structure](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/folder-structure.png)

### So let's start coding!

## Home Page

Write a basic HTML 5 template in ```index.html```. 

 Link ```home.css``` file to ```index.html``` file. 

 Now; create navbar.

```
<nav class="navbar">
<div class="nav">
    <img src="img/dark-logo.png" class="brand-logo" alt="">
    <div class="nav-items">
        <div class="search">
            <input type="text" class="search-box" placeholder="search brand, product">
            <button class="search-btn">search</button>
        </div>
        <a href="#"><img src="img/user.png" alt=""></a>
        <a href="#"><img src="img/cart.png" alt=""></a>
    </div>
</div>
</nav>
```

Open ```home.css``` file. 
* We'll have same navbar and footer in all pages. 
  * We're making styles as a separate file.

  * So import the `nav.js` file inside `home.css`.

```
@import 'nav.css';
```

* We will complete navbar related styles inside `nav.css`.

```
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'roboto', sans-serif;
}

.navbar{
    position: sticky;
    top: 0;
    left: 0;
    width: 100%;
    background: #f5f5f5;
    z-index: 9;
}

.nav{
    padding: 10px 10vw;
    display: flex;
    justify-content: space-between;
}

.brand-logo{
    height: 60px;
}

.nav-items{
    display: flex;
    align-items: center;
}

.search{
    width: 500px;
    display: flex;
}

.search-box{
    width: 80%;
    height: 40px;
    padding: 10px;
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
    border: 1px solid #d1d1d1;
    text-transform: capitalize;
    background: none;
    color: #a9a9a9;
    outline: none;
}

.search-btn{
    width: 20%;
    height: 40px;
    padding: 10px 20px;
    border: none;
    outline: none;
    cursor: pointer;
    background: #383838;
    color: #fff;
    text-transform: capitalize;
    font-size: 15px;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
}

.search-box::placeholder{
    color: #a9a9a9;
}

.nav-items a{
    margin-left: 20px;
}

.nav-items a img{
    width: 30px;
}

```






