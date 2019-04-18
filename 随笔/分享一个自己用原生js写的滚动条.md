```swift
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>document</title>
		<style>
			.kuang {
				width: 700px;
				height: 7px;
				/* background: #c03; */
				border: 1px solid #000;
				position: relative;
				border-radius: 7px;
				/* margin-left: 30%; */
				/* margin-top: 20%; */
				box-sizing: border-box;
				/*overflow: hidden;*/
			}
			.overflow {
				overflow: hidden;
				width: 700px;
				height: 7px;
				border-radius: 7px;;
				position: absolute;
				left: -1px;
				top: -1px;
			}
			.bg {
				position: absolute;
				background-color: pink;
				/* border-radius: 30px; */
				width: 700px;
				height: 9px;
				top: -1px;
				left: -100%;
			}
			.dot {
				position: absolute;
				display: inline-block;
				width: 16px;
				height: 16px;
				border-radius: 50%;
				background: pink;
				top: 50%;
				left: 0;
				transform: translate(-50%,-50%);
			}
		</style>
	</head>
	<body>
		<div class="kuang">
				<div class="overflow">
					<div class="bg"></div>
				</div>
				<a class="dot" draggable="false">
					<div class="line"></div>
				</a>
			</div>
			<script type="text/javascript">
				window.onload = function() {
					var kuang = document.querySelector(".kuang");
					var dot = document.querySelector('.dot');
					var bg = document.querySelector('.bg');
					var load1 = kuang.clientWidth * 0.1666;
					var load2 = kuang.clientWidth * 0.3333;
					var load3 = kuang.clientWidth * 0.5;
					var load4 = kuang.clientWidth * 0.6666;
					var load5 = kuang.clientWidth * 0.8333;
					var load6 = kuang.clientWidth;
					dot.onmousedown = function() {
						document.onmousemove = function(e) {
							var e = e || window.event;
							var x = e.clientX;
							var y = e.clientY;
							var mx = x - kuang.offsetLeft;
							var my = y - kuang.offsetTop;
							if(mx < load1) {
								dot.style.left = 0;
								bg.style.left = -load6 + 'px';
							} else if(mx > load1 && mx < load3) {
								dot.style.left = load2 + 'px';
								bg.style.left = -load4 + 'px';
							} else if(mx > load3 && mx < load5) {
								dot.style.left = load4 + 'px';
								bg.style.left = -load2 + 'px';
							} else if(mx > load5) {
								dot.style.left = load6 + 'px';
								bg.style.left = 0;
							}
							window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
						}
					}
					kuang.onclick = function(e) {
						var e = e || window.event;
						var x = e.clientX;
						var y = e.clientY;
						var mx = x - kuang.offsetLeft;
						var my = y - kuang.offsetTop;
						if(mx < load1) {
							dot.style.left = 0;
							bg.style.left = -load6 + 'px';
						} else if(mx > load1 && mx < load3) {
							dot.style.left = load2 + 'px';
							bg.style.left = -load4 + 'px';
						} else if(mx > load3 && mx < load5) {
							dot.style.left = load4 + 'px';
							bg.style.left = -load2 + 'px';
						} else if(mx > load5) {
							dot.style.left = load6 + 'px';
							bg.style.left = 0;
						}
					}
					document.onmouseup = function() {
						document.onmousemove = null;
					}
					/*移动端try*/
					function touchmove(event) {
						var touch = event.targetTouches[0];
						var mx = touch.pageX - kuang.offsetLeft;
						if(event.targetTouches.length == 1) {　　　　
							event.preventDefault();
							if(mx < load1) {
								dot.style.left = 0;
								bg.style.left = -load6 + 'px';
							} else if(mx > load1 && mx < load3) {
								dot.style.left = load2 + 'px';
								bg.style.left = -load4 + 'px';
							} else if(mx > load3 && mx < load5) {
								dot.style.left = load4 + 'px';
								bg.style.left = -load2 + 'px';
							} else if(mx > load5) {
								dot.style.left = load6 + 'px';
								bg.style.left = 0;
							}
						}
					}

					function touchstart(e) {
						kuang.addEventListener('touchmove', touchmove, false);
					}
					dot.addEventListener("touchstart", touchstart, false);
					document.addEventListener("touchend", function() {
					  //dot.removeEventListener("touchstart",touchstart,false);  
						kuang.removeEventListener("touchmove", touchmove, false);
					});
				}
			</script>
	</body>
</html>

```
其中有注意的问题！！！
```
window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();  //实现元素拖动时，出现禁止图标。
```