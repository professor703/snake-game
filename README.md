# snake-game
this is java script code for the snake game 
document.body.innerHTML = `
    <div style="background-color:#1a1a1a;color:#fff;font-family:sans-serif;display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;margin:0;">
        <h1>Snake Game</h1>
        <div style="font-size:20px;margin-bottom:15px;">Score: <span id="score">0</span></div>
        <canvas id="gameCanvas" width="400" height="400" style="border:4px solid #fff;background:#000;"></canvas>
    </div>`;

const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");
const scoreElement = document.getElementById("score");

const gridSize = 20;
const tileCount = canvas.width / gridSize;
let snake = [{ x: 10, y: 10 }, { x: 9, y: 10 }, { x: 8, y: 10 }];
let food = { x: 5, y: 5 };
let dx = 1, dy = 0, score = 0, gameInterval;
let changingDirection = false;

function startGame() {
    resetGame();
    if (gameInterval) clearInterval(gameInterval);
    gameInterval = setInterval(gameLoop, 100);
}
function gameLoop() {
    changingDirection = false;
    ctx.fillStyle = "#000";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "#FF5722";
    ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 2, gridSize - 2);
    
    const head = { x: snake[0].x + dx, y: snake[0].y + dy };
    snake.unshift(head);
    if (head.x === food.x && head.y === food.y) {
        score += 10;
        scoreElement.innerText = score;
        food.x = Math.floor(Math.random() * tileCount);
        food.y = Math.floor(Math.random() * tileCount);
    } else {
        snake.pop();
    }
    
    snake.forEach((part, index) => {
        ctx.fillStyle = index === 0 ? "#4CAF50" : "#8BC34A";
        ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize - 2, gridSize - 2);
    });
    
    if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
        clearInterval(gameInterval);
        alert("Game Over! Score: " + score);
    }
    for (let i = 1; i < snake.length; i++) {
        if (head.x === snake[i].x && head.y === snake[i].y) {
            clearInterval(gameInterval);
            alert("Game Over! Score: " + score);
        }
    }
}
function resetGame() {
    snake = [{ x: 10, y: 10 }, { x: 9, y: 10 }, { x: 8, y: 10 }];
    dx = 1; dy = 0; score = 0; scoreElement.innerText = score;
}
document.addEventListener("keydown", e => {
    if (changingDirection) return;
    if (e.key === "ArrowLeft" && dx === 0) { dx = -1; dy = 0; changingDirection = true; }
    if (e.key === "ArrowUp" && dy === 0) { dx = 0; dy = -1; changingDirection = true; }
    if (e.key === "ArrowRight" && dx === 0) { dx = 1; dy = 0; changingDirection = true; }
    if (e.key === "ArrowDown" && dy === 0) { dx = 0; dy = 1; changingDirection = true; }
});
startGame();
