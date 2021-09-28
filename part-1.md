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
> OUTPUT
![Navbar](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/navbar.png)


* Now we create links below the navbar.

```
<ul class="links-container">
    <li class="link-item"><a href="#" class="link">home</a></li>
    <li class="link-item"><a href="#" class="link">women</a></li>
    <li class="link-item"><a href="#" class="link">men</a></li>
    <li class="link-item"><a href="#" class="link">kids</a></li>
    <li class="link-item"><a href="#" class="link">accessories</a></li>
</ul>

```
^ The above code is inside the navbar element. ^

```
.links-container{
    width: 100%;
    display: flex;
    padding: 10px 10vw;
    justify-content: center;
    list-style: none;
    border-top: 1px solid #d1d1d1;
}

.link{
    text-transform: capitalize;
    padding: 0 10px;
    margin: 0 5px;
    text-decoration: none;
    color: #383838;
    opacity: 0.5;
    transition: .5s;
}

.link:hover{
    opacity: 1;
}

```

> OUTPUT
![](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/navbar2.png)

Great! Now we want the navbar on all pages. I am don't like to copy the code. So 
  * Let's make this navbar with JS dynamically. 
   
   * Open the `nav.js` file. 
   
   * Make a `createNav` function inside it.

```
const createNav = () => {
    let nav = document.querySelector('.navbar');

    nav.innerHTML = `
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
        <ul class="links-container">
            <li class="link-item"><a href="#" class="link">home</a></li>
            <li class="link-item"><a href="#" class="link">women</a></li>
            <li class="link-item"><a href="#" class="link">men</a></li>
            <li class="link-item"><a href="#" class="link">kids</a></li>
            <li class="link-item"><a href="#" class="link">accessories</a></li>
        </ul>
    `;
}

```

In the above code, inside the function, we select the nav element using the `querySelector` method. 

* Then write its HTML using `innerHTML`. The value of `innerHTML` is the same HTML elements we have made in the `index.html` file. 

* Now remove the HTML elements from there and import `nav.js`.

```
<nav class="navbar"></nav>
<script src="js/nav.js"></script>
```
> OUTPUT
![](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/navbar3.png)

Now let's make its header.

```
<!-- hero section -->
<header class="hero-section">
    <div class="content">
        <img src="img/light-logo.png" class="logo" alt="">
        <p class="sub-heading">best fashion collection of all time</p>
    </div>
</header>
```

> `Home.css`

```
@import 'nav.css';

.hero-section{
    width: 100%;
    height: calc(100vh - 120px);
    background-image: url('../img/header.png');
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
}

.hero-section .logo{
    height: 150px;
    display: block;
    margin: auto;
}

.hero-section .sub-heading{
    margin-top: 10px;
    text-align: center;
    color: #fff;
    text-transform: capitalize;
    font-size: 35px;
    font-weight: 300;
}
```
> OUTPUT
![](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/clothing1.jpeg)

Now we make a product card slider. 

> `index.html`

```
<section class="product">
    <h2 class="product-category">best selling</h2>
</section>
```


> Home.css

```
.product{
    position: relative;
    overflow: hidden;
    padding: 20px 0;
}

.product-category{
    padding: 0 10vw;
    font-size: 30px;
    font-weight: 500;
    margin-bottom: 40px;
    text-transform: capitalize;
}
```

> OUTPUT
![](https://github.com/DrVicki/ecommerce-guided-learning/blob/main/img/clothing2.jpeg)

Now we make a product card.

```
// inside product section.
<div class="product-container">
    <div class="product-card">
        <div class="product-image">
            <span class="discount-tag">50% off</span>
            <img src="img/card1.png" class="product-thumb" alt="">
            <button class="card-btn">add to whislist</button>
        </div>
        <div class="product-info">
            <h2 class="product-brand">brand</h2>
            <p class="product-short-des">a short line about the cloth..</p>
            <span class="price">$20</span><span class="actual-price">$40</span>
        </div>
    </div>
    +7 more cards
</div>
```

 * We'll make these product cards with JS and database dynamically later.

> Home.css

```
.product-container{
    padding: 0 10vw;
    display: flex;
    overflow-x: auto;
    scroll-behavior: smooth;
}

.product-container::-webkit-scrollbar{
    display: none;
}

.product-card{
    flex: 0 0 auto;
    width: 250px;
    height: 450px;
    margin-right: 40px;
}

.product-image{
    position: relative;
    width: 100%;
    height: 350px;
    overflow: hidden;
}

.product-thumb{
    width: 100%;
    height: 350px;
    object-fit: cover;
}

.discount-tag{
    position: absolute;
    background: #fff;
    padding: 5px;
    border-radius: 5px;
    color: #ff7d7d;
    right: 10px;
    top: 10px;
    text-transform: capitalize;
}

.card-btn{
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    padding: 10px;
    width: 90%;
    text-transform: capitalize;
    border: none;
    outline: none;
    background: #fff;
    border-radius: 5px;
    transition: 0.5s;
    cursor: pointer;
    opacity: 0;
}

.product-card:hover .card-btn{
    opacity: 1;
}

.card-btn:hover{
    background: #efefef;
}

.product-info{
    width: 100%;
    height: 100px;
    padding-top: 10px;
}

.product-brand{
    text-transform: uppercase;
}

.product-short-des{
    width: 100%;
    height: 20px;
    line-height: 20px;
    overflow: hidden;
    opacity: 0.5;
    text-transform: capitalize;
    margin: 5px 0;
}

.price{
    font-weight: 900;
    font-size: 20px;
}

.actual-price{
    margin-left: 20px;
    opacity: 0.5;
    text-decoration: line-through;
}
```
