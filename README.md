<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }

        canvas {
            border: 1px solid #000;
        }
    </style>
</head>
<body>

<canvas id="snakeCanvas" width="400" height="400"></canvas>

<script>
    const canvas = document.getElementById("snakeCanvas");
    const ctx = canvas.getContext("2d");

    const boxSize = 20;
    let score = 0;
    let snake = [{ x: 10, y: 10 }];
    let food = { x: 15, y: 15 };
    let dx = 1;
    let dy = 0;

    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Draw snake
        ctx.fillStyle = "#00F";
        for (let i = 0; i < snake.length; i++) {
            ctx.fillRect(snake[i].x * boxSize, snake[i].y * boxSize, boxSize, boxSize);
        }

        // Draw food
        ctx.fillStyle = "#F00";
        ctx.fillRect(food.x * boxSize, food.y * boxSize, boxSize, boxSize);

        // Move snake
        const newHead = { x: snake[0].x + dx, y: snake[0].y + dy };
        snake.unshift(newHead);

        // Check if snake eats food
        if (newHead.x === food.x && newHead.y === food.y) {
            score++;
            generateFood();
        } else {
            snake.pop();
        }

        // Check for collisions
        if (
            newHead.x < 0 ||
            newHead.y < 0 ||
            newHead.x >= canvas.width / boxSize ||
            newHead.y >= canvas.height / boxSize ||
            checkCollision(newHead, snake.slice(1))
        ) {
            alert("Game Over! Your score: " + score);
            resetGame();
        }

        // Display score
        ctx.fillStyle = "#000";
        ctx.font = "20px Arial";
        ctx.fillText("Score: " + score, 10, 30);
    }

    function generateFood() {
        food = {
            x: Math.floor(Math.random() * (canvas.width / boxSize)),
            y: Math.floor(Math.random() * (canvas.height / boxSize))
        };

        // Ensure food does not spawn on the snake
        for (let i = 0; i < snake.length; i++) {
            if (food.x === snake[i].x && food.y === snake[i].y) {
                generateFood();
                break;
            }
        }
    }

    function checkCollision(head, array) {
        for (let i = 0; i < array.length; i++) {
            if (head.x === array[i].x && head.y === array[i].y) {
                return true;
            }
        }
        return false;
    }

    function resetGame() {
        snake = [{ x: 10, y: 10 }];
        score = 0;
        dx = 1;
        dy = 0;
        generateFood();
    }

    document.addEventListener("keydown", direction);

    function direction(event) {
        if (event.keyCode === 37 && dx === 0) {
            dx = -1;
            dy = 0;
        } else if (event.keyCode === 39 && dx === 0) {
            dx = 1;
            dy = 0;
        } else if (event.keyCode === 38 && dy === 0) {
            dx = 0;
            dy = -1;
        } else if (event.keyCode === 40 && dy === 0) {
            dx = 0;
            dy = 1;
        }
    }

    setInterval(draw, 100);
</script>

</body>
</html>
