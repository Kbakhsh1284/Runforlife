```
<!DOCTYPE html>
<html>
<head>
	<title>Running Game</title>
	<style>
		/* Add some basic styling to our game */
		canvas {
			border: 1px solid black;
		}
		.score {
			font-size: 24px;
			font-weight: bold;
			margin-bottom: 10px;
		}
		.lives {
			font-size: 24px;
			font-weight: bold;
			margin-bottom: 10px;
		}
	</style>
</head>
<body>
	<canvas id="gameCanvas" width="400" height="200"></canvas>
	<div class="score">Score: 0</div>
	<div class="lives">Lives: 3</div>
	<script>
		// Get the canvas element
		var canvas = document.getElementById('gameCanvas');
		var ctx = canvas.getContext('2d');

		// Set the canvas dimensions
		canvas.width = 400;
		canvas.height = 200;

		// Define the player and obstacle objects
		var player = {
			x: 50,
			y: 150,
			width: 20,
			height: 20,
			speed: 5
		};

		var obstacles = [];
		var obstacleInterval = 1000; // milliseconds
		var lastObstacleTime = 0;

		// Define the score and lives
		var score = 0;
		var lives = 3;

		// Draw the player and obstacles on the canvas
		function draw() {
			ctx.clearRect(0, 0, canvas.width, canvas.height);
			ctx.fillStyle = 'blue';
			ctx.fillRect(player.x, player.y, player.width, player.height);
			for (var i = 0; i < obstacles.length; i++) {
				ctx.fillStyle = 'red';
				ctx.fillRect(obstacles[i].x, obstacles[i].y, obstacles[i].width, obstacles[i].height);
			}
			document.querySelector('.score').innerHTML = 'Score: ' + score;
			document.querySelector('.lives').innerHTML = 'Lives: ' + lives;
		}

		// Update the player and obstacle positions
		function update() {
			// Move the player
			if (keys['ArrowLeft']) {
				player.x -= player.speed;
			}
			if (keys['ArrowRight']) {
				player.x += player.speed;
			}

			// Move the obstacles
			for (var i = 0; i < obstacles.length; i++) {
				obstacles[i].x -= obstacles[i].speed;
				if (obstacles[i].x < 0) {
					obstacles.splice(i, 1);
					score++;
				}
			}

			// Check for collision
			for (var i = 0; i < obstacles.length; i++) {
				if (checkCollision(player, obstacles[i])) {
					lives--;
					obstacles.splice(i, 1);
					if (lives === 0) {
						alert('Game Over! Your score: ' + score);
						location.reload();
					}
				}
			}

			// Add new obstacles
			var now = Date.now();
			if (now - lastObstacleTime > obstacleInterval) {
				lastObstacleTime = now;
				obstacles.push({
					x: canvas.width,
					y: 150,
					width: 20,
					height: 20,
					speed: 2
				});
			}
		}

		// Check for collision between two objects
		function checkCollision(obj1, obj2) {
			if (obj1.x < obj2.x + obj2.width &&
				obj1.x + obj1.width > obj2.x &&
				obj1.y < obj2.y + obj2.height &&
				obj1.y + obj1.height > obj2.y) {
				return true;
			}
			return false;
		}

		// Handle keyboard input
		var keys = {};
		document.addEventListener('keydown', function(event) {
			keys[event.key] = true;
		});
		document.addEventListener('keyup', function(event) {
			keys[event.key] = false;
		});

		// Main game loop
		setInterval(function() {
			update();
			draw();
		}, 16); // 16ms = approximately 60fps
	</script>
</body>
</html>
```
