---
date: 2025-08-26
---

## 代码
=== "index.html"
	
	```html
	<html lang="en">  
	<head>  
	    <meta charset="UTF-8">  
	    <meta name="viewport" content="width=device-width, initial-scale=1">  
	    <title>Document</title>  
	    <link rel="stylesheet" href="style.css">  
	</head>  
	<body>  
	    <div class="container">  
	        <div class="progress-container">  
	            <div id="progress"></div>  
	            <div class="circle active">1</div>  
	            <div class="circle">2</div>  
	            <div class="circle">3</div>  
	            <div class="circle">4</div>  
	        </div>        <button class="prev" disabled>prev</button>  
	        <button class="next">next</button>  
	    </div>    <script src="script.js"></script>  
	</body>  
	</html>
	```

=== "style.css"
	
	```css
	:root {  
	    --line-border-empty: #e0e0e0;  
	    --line-border-fill: #34db90;  
	}  
	  
	  
	body {  
	    margin: 0;  
	    width: 100vw;  
	    height: 100vh;  
	    display: flex;  
	    justify-content: center;  
	    align-items: center;  
	    font-family: sans-serif;  
	    background-color: #f7f7fa;  
	}  
	  
	.container {  
	    text-align: center;  
	}  
	  
	.progress-container {  
	    display: flex;  
	    width: 300px;  
	    justify-content: space-between;  
	    position: relative;  
	}  
	  
	.progress-container:before {  
	    content: '';  
	    position: absolute;  
	    height: 5px;  
	    width: 100%;  
	    top: 50%;  
	    transform: translateY(-50%);  
	    background-color: var(--line-border-empty);  
	    z-index: -1;  
	}  
	  
	#progress {  
	    position: absolute;  
	    height: 5px;  
	    width: 0;  
	    top: 50%;  
	    transform: translateY(-50%);  
	    background-color: var(--line-border-fill);  
	    z-index: -1;  
	    transition: width ease-out .5s;  
	}  
	  
	.circle {  
	    height: 30px;  
	    width: 30px;  
	    display: flex;  
	    justify-content: center;  
	    align-items: center;  
	    background-color: white;  
	    border: 3px solid var(--line-border-empty);  
	    border-radius: 50%;  
	    padding: 10px;  
	    font-size: 14px;  
	    color: gray;  
	    transition: border-color ease-out .5s;  
	}  
	  
	button {  
	    color: white;  
	    background-color: #2083ac;  
	    border: none;  
	    border-radius: 5px;  
	    margin: 20px 10px;  
	    padding: 5px 20px;  
	    cursor: pointer;  
	}  
	  
	button:active {  
	    transform: scale(0.98);  
	}  
	  
	button:disabled {  
	    background-color: #383838;  
	    cursor: not-allowed;  
	}  
	  
	.circle.active {  
	    border-color: var(--line-border-fill);  
	}  
	  
	* {  
	    box-sizing: border-box;  
	}
	```

=== "script.js"
	
	```js
	const prev = document.querySelector('.prev');  
	const next = document.querySelector('.next');  
	const circles = document.querySelectorAll('.circle');  
	const progress = document.querySelector('#progress')  
	  
	let cur = 0;  
	const max_cur = 3;  
	const min_cur = 0;  
	  
	next.addEventListener('click', () => {  
	    cur + 1 <= max_cur && cur++;  
	    update();  
	})  
	  
	prev.addEventListener('click', () => {  
	    cur - 1 >= min_cur && cur--;  
	    update();  
	})  
	  
	function update() {  
	    circles.forEach(circle => circle.classList.remove('active'))  
	    for (let i = 0; i <= cur; i ++) {  
	        circles[i].classList.add('active')  
	    }  
	    progress.style.width = (cur / max_cur * 100) + '%';  
	  
	    cur === max_cur ? next.disabled = true : next.disabled = false;  
	    cur === min_cur ? prev.disabled = true : prev.disabled = false;  
	}
	```

<!-- more -->
## 总结

没什么难点，通读一遍代码就能明白其实现原理。进度条通过伪类元素 `::after` 搭配绝对定位实现。进度条的进度通过实时计算当前进度的步骤 `progress.style.width = (cur/max_cur*100)+'%'` 实现，再添加上 `transition` 效果就有了很丝滑的进度条动画。

效果图：

![image.png](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202508260405198.png)
