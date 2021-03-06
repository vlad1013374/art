<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		body{
			margin:0;
			display: block;
			background-color: #CB77D7;
		}
		nav{
			position: absolute;
			display: block;
			right: 0;
			width: 200px;
		}
		.menu-item{
			display: inline-block;
			color:#333333;
			font-family: Arial;
			font-size: 14px;
			line-height: 30px;
			text-decoration: none;
			text-transform: uppercase;
			padding: 0 10px;
			float: right;
			position: relative;
		}

	</style>
</head>
<body>
	
	<nav>
	<div><a href="#" class="menu-item">INSTRUCTION</a></div>
	<div><a href="Menu.html" class="menu-item">HOME</a></div>
	</nav>
	<canvas id="canvas">Your bro</canvas>	
	<script>
		var canv = document.getElementById('canvas');
		var ctx = canv.getContext('2d');
		var isMouseDown = false;
		var coords = [];
		var radius = 10;

		canv.width = window.innerWidth;
		canv.height = window.innerHeight;


		//Code
		canv.addEventListener('mousedown', function () {
			isMouseDown = true;
		});
		canv.addEventListener('mouseup', function () {
			isMouseDown = false;
			ctx.beginPath();
			coords.push('mouseup');
		});
		ctx.lineWidth = radius*2;
		canv.addEventListener('mousemove', function (e) {

			if (isMouseDown) 
			{
			coords.push([e.clientX, e.clientY]);

			ctx.lineTo(e.clientX, e.clientY);
			ctx.stroke();

			ctx.beginPath();
			ctx.arc(e.clientX, e.clientY, radius, 0, Math.PI * 2);
			ctx.fill();

			ctx.beginPath();
			ctx.moveTo(e.clientX, e.clientY);
			}
		});

		function save(){
			localStorage.setItem('coords', JSON.stringify(coords));
		}

		function clear(){
			ctx.fillStyle = '#CB77D7';
			ctx.fillRect(0,0,canv.width, canv.height);

			ctx.beginPath();
			ctx.fillStyle = 'black';
		}

		function play(){
			var timer = setInterval(function(){
				if(!coords.length){
					clearInterval(timer);
					ctx.beginPath();
					return;
				}

				var 
					crd = coords.shift(),
					e = {
						clientX: crd['0'],
						clientY: crd['1']
					}

					ctx.lineTo(e.clientX, e.clientY);
			ctx.stroke();

			ctx.beginPath();
			ctx.arc(e.clientX, e.clientY, 10, 0, Math.PI * 2);
			ctx.fill();

			ctx.beginPath();
			ctx.moveTo(e.clientX, e.clientY);
			},30);
		}

		document.addEventListener('keydown', function(e){
			console.log(e.keyCode);
			if(e.keyCode == 83){
				save();
				console.log('Saved');
			}

			if(e.keyCode == 82){
				//play

				console.log('Replaying');

				coords = JSON.parse(localStorage.getItem('coords'));
				clear();
				play();
			}

			if(e.keyCode == 67){
				clear();
				console.log('Cleared');
			}
		});




	</script>
</body>
</html>
