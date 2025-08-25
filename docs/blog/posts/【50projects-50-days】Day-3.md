---
date: 2025-08-26
---
## 代码

=== "index.html"

	```html
	<!DOCTYPE html>  
	<html lang="en">  
	<head>  
	    <meta charset="UTF-8">  
	    <title>Title</title>  
	    <link rel="stylesheet" href="style.css">  
	    <script src="https://kit.fontawesome.com/18093c370e.js" crossorigin="anonymous"></script>  
	</head>  
	<body>  
	    <div class="container">  
	        <div class="circle-container">  
	            <div class="circle">  
	                <button id="open">  
	                    <i class="fa-solid fa-list"></i>  
	                </button>                <button id="close">  
	                    <i class="fa-solid fa-xmark"></i>  
	                </button>            </div>        </div>        <div class="content">  
	            <h1>Amazing Article</h1>  
	            <small>Florin Pop</small>  
	            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Accusantium quia in ratione dolores cupiditate, maxime aliquid impedit dolorem nam dolor omnis atque fuga labore modi veritatis porro laborum minus, illo, maiores recusandae cumque ipsa quos. Tenetur, consequuntur mollitia labore pariatur sunt quia harum aut. Eum maxime dolorem provident natus veritatis molestiae cumque quod voluptates ab non, tempore cupiditate? Voluptatem, molestias culpa. Corrupti, laudantium iure aliquam rerum sint nam quas dolor dignissimos in error placeat quae temporibus minus optio eum soluta cupiditate! Cupiditate saepe voluptates laudantium. Ducimus consequuntur perferendis consequatur nobis exercitationem molestias fugiat commodi omnis. Asperiores quia tenetur nemo ipsa.</p>  
	  
	            <h3>My Dog</h3>  
	            <img src="https://images.unsplash.com/photo-1507146426996-ef05306b995a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2100&q=80" alt="doggy" />  
	            <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Sit libero deleniti rerum quo, incidunt vel consequatur culpa ullam. Magnam facere earum unde harum. Ea culpa veritatis magnam at aliquid. Perferendis totam placeat molestias illo laudantium? Minus id minima doloribus dolorum fugit deserunt qui vero voluptas, ut quia cum amet temporibus veniam ad ea ab perspiciatis, enim accusamus asperiores explicabo provident. Voluptates sint, neque fuga cum illum, tempore autem maxime similique laborum odio, magnam esse. Aperiam?</p>  
	        </div>    </div>    <nav>        <ul>            <li><a href="#">Home</a></li>  
	            <li><a href="#">About</a></li>  
	            <li><a href="#">Archive</a></li>  
	        </ul>    </nav>    <script src="script.js"></script>  
	</body>  
	</html>
	```

=== "style.css"

	```css
	@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap');  
	  
	body {  
	    margin: 0;  
	    overflow-x: hidden;  
	    position: relative;  
	    background-color: #333;  
	}  
	  
	* {  
	    box-sizing: border-box;  
	}  
	  
	.container {  
	    width: 100vw;  
	    padding: 50px;  
	    background-color: white;  
	    transform-origin: top left;  
	    transition: transform 0.5s linear;  
	}  
	  
	.circle-container {  
	    position: fixed;  
	    left: -100px;  
	    top: -100px;  
	}  
	  
	.circle {  
	    width: 200px;  
	    height: 200px;  
	    background-color: hotpink;  
	    border-radius: 50%;  
	    position: relative;  
	    transition: tranform 0.5s linear;  
	    transform-origin: left top;  
	}  
	  
	.circle button {  
	    position: absolute;  
	    cursor: pointer;  
	    left: 50%;  
	    top: 50%;  
	    height: 100px;  
	    transform-origin: left top;  
	    background: transparent;  
	    border: 0;  
	    font-size: 26px;  
	    transition: transform 0.5s linear;  
	}  
	  
	i {  
	    color: white;  
	}  
	  
	#open {  
	    left: 60%;  
	}  
	  
	#close {  
	    transform: rotate(90deg);  
	    top: 55%;  
	}  
	  
	.container.nav-open button#open {  
	    transform: rotate(-90deg);  
	}  
	  
	.container.nav-open button#close {  
	    transform: rotate(0);  
	}  
	  
	.container.nav-open {  
	    transform: rotate(-10deg);  
	}  
	  
	.content {  
	    max-width: 1000px;  
	    margin: 50px auto;  
	    font-family: "Roboto", sans-serif;  
	    line-height: 1.5;  
	    font-size: 14px;  
	    color: #222;  
	}  
	  
	h1 {  
	    margin: 0;  
	    font-size: 25px;  
	}  
	  
	small {  
	    font-style: italic;  
	}  
	  
	img {  
	    max-width: 100%;  
	}  
	  
	nav {  
	    position: absolute;  
	    left: -100px;  
	    bottom: 0;  
	    transition: left 1s ease-in-out;  
	}  
	  
	.container.nav-open+nav {  
	    left: 0;  
	}  
	  
	nav>ul {  
	    list-style: none;  
	}  
	  
	nav>ul>li>a {  
	    text-decoration: none;  
	    color: white;  
	}  
	  
	nav>ul>li>a:hover {  
	    color: coral;  
	}
	```

=== "script.js"

	```js
	const open = document.querySelector('#open')  
	const close = document.querySelector('#close')  
	const container = document.querySelector('.container')  
	  
	open.addEventListener('click', () => {  
	    container.classList.add('nav-open')  
	})  
	  
	close.addEventListener('click', () => {  
	    container.classList.remove('nav-open')  
	})
	```

<!-- more -->

## 总结

本 project 难点在于左上角 `circle` 及图标的旋转处理，需要对 `transform-origin`，`transform` 有一定的理解。

效果：

![image.png](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202508260602183.png)


