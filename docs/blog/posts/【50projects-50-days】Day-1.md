---
date: 2025-08-26
---
## 代码

=== "script.js"

	```js
	const panels = document.querySelectorAll('.panel')  
	  
	panels.forEach(panel => {  
	    panel.addEventListener('click', () => {  
	        removeActive()  
	        panel.classList.add('active')  
	    })  
	})  
	  
	function removeActive() {  
	    panels.forEach(panel => {  
	        panel.classList.remove('active')  
	    })  
	}
	```

=== "style.css"
	
	```css
	body {  
	    display: flex;  
	    justify-content: center;  
	    align-items: center;  
	    height: 100vh;  
	    margin: 0;  
	    background-color: black;  
	}  
	  
	.container {  
	    display: flex;  
	    width: 90vw;  
	}  
	  
	.panel {  
	    background-position: center;  
	    background-size: cover;  
	    color: white;  
	    height: 80vh;  
	    border-radius: 50px;  
	    flex: 0.5;  
	    cursor: pointer;  
	    margin: 10px;  
	    transition: flex ease-in-out 1s;  
	    position: relative;  
	    overflow: hidden;  
	}  
	  
	.panel h2 {  
	    position: absolute;  
	    font-size: 24px;  
	    left: 5px;  
	    bottom: 0;  
	    opacity: 0;  
	    text-wrap: nowrap;  
	}  
	  
	@media screen and (max-width: 800px) {  
	    .panel:nth-of-type(5) {  
	        display: none;  
	    }  
	}  
	  
	.active {  
	    flex: 2;  
	}  
	  
	.active h2 {  
	    opacity: 1;  
	    transition: opacity ease 2s;  
	}
	```
=== "script.js"
	
	```js
	const panels = document.querySelectorAll('.panel')  
	  
	panels.forEach(panel => {  
	    panel.addEventListener('click', () => {  
	        removeActive()  
	        panel.classList.add('active')  
	    })  
	})  
	  
	function removeActive() {  
	    panels.forEach(panel => {  
	        panel.classList.remove('active')  
	    })  
	}
	```


<!-- more -->

## 总结

布局上没有很大的难度，该project的特色在于通过改变 `flex: xx; ` 实现了卡片之间丝滑的大小切换，做完有一定收获。

效果：

![image.png](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202508260220491.png)
