# Ganesh jii
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modak Catch - गणपति बप्पा मोरया</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            background: linear-gradient(135deg, #8B0000 0%, #DC143C 50%, #FF6B6B 100%);
        }

        #gameContainer {
            width: 100%;
            height: 100%;
            position: relative;
            overflow: hidden;
            background: 
                linear-gradient(90deg, rgba(255, 215, 0, 0.1) 0%, transparent 10%, transparent 90%, rgba(255, 215, 0, 0.1) 100%),
                linear-gradient(180deg, #FFD700 0%, #FFA500 50%, #FF6347 100%);
        }

        /* Pandal-style decorative borders */
        #gameContainer::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 40px;
            background: repeating-linear-gradient(90deg, #FFD700, #FFD700 20px, #FF8C00 20px, #FF8C00 40px);
            z-index: 1;
        }

        #gameContainer::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 40px;
            background: repeating-linear-gradient(90deg, #FFD700, #FFD700 20px, #FF8C00 20px, #FF8C00 40px);
            z-index: 1;
        }

        /* Rangoli-style side decorations */
        .rangoli-left {
            position: absolute;
            left: 0;
            top: 0;
            width: 30px;
            height: 100%;
            background: repeating-linear-gradient(180deg, #FFD700, #FFD700 30px, #8B0000 30px, #8B0000 60px);
            z-index: 0;
        }

        .rangoli-right {
            position: absolute;
            right: 0;
            top: 0;
            width: 30px;
            height: 100%;
            background: repeating-linear-gradient(180deg, #FFD700, #FFD700 30px, #8B0000 30px, #8B0000 60px);
            z-index: 0;
        }

        /* Screen overlays */
        .screen {
            position: absolute;
            width: 100%;
            height: 100%;
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 100;
            background: rgba(0, 0, 0, 0.7);
        }

        .screen.active {
            display: flex;
        }

        .screen-content {
            text-align: center;
            background: linear-gradient(135deg, #8B0000, #DC143C);
            padding: 40px;
            border-radius: 20px;
            color: white;
            border: 4px solid #FFD700;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
            max-width: 90%;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from {
                transform: scale(0.8);
                opacity: 0;
            }
            to {
                transform: scale(1);
                opacity: 1;
            }
        }

        .screen-content h1 {
            font-size: 3em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            color: #FFD700;
        }

        .screen-content h2 {
            font-size: 2em;
            margin-bottom: 15px;
            color: #FFD700;
        }

        .screen-content p {
            font-size: 1.2em;
            margin-bottom: 10px;
            color: #FFF;
        }

        .festive-greeting {
            font-size: 1.8em;
            font-weight: bold;
            margin: 20px 0;
            color: #FFD700;
            font-family: 'Georgia', serif;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        button {
            margin-top: 20px;
            padding: 15px 40px;
            font-size: 1.2em;
            background: linear-gradient(135deg, #FFD700, #FFA500);
            border: none;
            border-radius: 10px;
            color: #8B0000;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
            background: linear-gradient(135deg, #FFA500, #FF8C00);
        }

        button:active {
            transform: translateY(0);
        }

        /* Game UI */
        #gameUI {
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            display: flex;
            justify-content: space-between;
            z-index: 50;
            color: white;
            font-size: 1.5em;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .ui-item {
            background: rgba(0, 0, 0, 0.5);
            padding: 10px 20px;
            border-radius: 8px;
            border: 2px solid #FFD700;
        }

        /* Player basket */
        #basket {
            position: absolute;
            bottom: 20px;
            width: 60px;
            height: 50px;
            background: linear-gradient(135deg, #8B4513, #D2691E);
            border-radius: 0 0 20px 20px;
            border: 3px solid #654321;
            left: calc(50% - 30px);
            z-index: 10;
        }

        /* Basket decorative handles */
        #basket::before {
            content: '';
            position: absolute;
            top: -10px;
            left: 5px;
            right: 5px;
            height: 15px;
            border: 3px solid #654321;
            border-bottom: none;
            border-radius: 20px 20px 0 0;
        }

        /* Falling modaks and obstacles */
        .modak {
            position: absolute;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #FFE4B5, #DAA520);
            border: 2px solid #CD853F;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            z-index: 5;
        }

        .obstacle {
            position: absolute;
            width: 35px;
            height: 35px;
            background: linear-gradient(135deg, #556B2F, #6B8E23);
            border-radius: 50%;
            border: 2px solid #3d4d1f;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            z-index: 5;
        }

        /* Floating text (combo/score pop-ups) */
        .floatingText {
            position: absolute;
            color: #FFD700;
            font-weight: bold;
            font-size: 1.5em;
            pointer-events: none;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            z-index: 20;
            animation: floatUp 1s ease-out forwards;
        }

        @keyframes floatUp {
            to {
                transform: translateY(-60px);
                opacity: 0;
            }
        }

        /* Mobile controls */
        .mobile-controls {
            position: absolute;
            bottom: 80px;
            left: 10px;
            right: 10px;
            display: none;
            justify-content: space-between;
            z-index: 30;
            gap: 20px;
        }

        .mobile-controls.active {
            display: flex;
        }

        .control-button {
            flex: 1;
            padding: 20px;
            background: rgba(255, 215, 0, 0.7);
            border: 3px solid #FFD700;
            border-radius: 10px;
            font-size: 1.5em;
            font-weight: bold;
            color: #8B0000;
            cursor: pointer;
            user-select: none;
            touch-action: manipulation;
        }

        .control-button:active {
            background: rgba(255, 215, 0, 0.9);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .screen-content {
                padding: 30px;
            }

            .screen-content h1 {
                font-size: 2em;
            }

            .screen-content h2 {
                font-size: 1.5em;
            }

            .festive-greeting {
                font-size: 1.3em;
            }

            button {
                padding: 12px 30px;
                font-size: 1em;
            }
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <div class="rangoli-left"></div>
        <div class="rangoli-right"></div>

        <!-- Start Screen -->
        <div id="startScreen" class="screen active">
            <div class="screen-content">
                <h1>🧡 Modak Catch 🧡</h1>
                <div class="festive-greeting">गणपति बप्पा मोरया</div>
                <p>Catch the modaks and celebrate Ganesh Chaturthi!</p>
                <p style="font-size: 1em; margin-top: 15px; color: #FFD700;">
                    Use arrow keys or A/D to move.<br>
                    Mobile: Drag or use buttons. Catch modaks, avoid thorns!
                </p>
                <button onclick="startGame()">Play</button>
            </div>
        </div>

        <!-- Game Over Screen -->
        <div id="gameOverScreen" class="screen">
            <div class="screen-content">
                <h2>Game Over!</h2>
                <p style="font-size: 2em; color: #FFD700; margin: 20px 0;">Final Score: <span id="finalScore">0</span></p>
                <div class="festive-greeting">गणपति बप्पा मोरया</div>
                <button onclick="startGame()">Play Again</button>
            </div>
        </div>

        <!-- Game UI -->
        <div id="gameUI" style="display: none;">
            <div class="ui-item">Score: <span id="score">0</span></div>
            <div class="ui-item">Lives: <span id="lives">3</span>/3</div>
        </div>

        <!-- Mobile Controls -->
        <div class="mobile-controls" id="mobileControls">
            <button class="control-button" ontouchstart="moveLeft = true;" ontouchend="moveLeft = false;" onmousedown="moveLeft = true;" onmouseup="moveLeft = false;">◀ LEFT</button>
            <button class="control-button" ontouchstart="moveRight = true;" ontouchend="moveRight = false;" onmousedown="moveRight = true;" onmouseup="moveRight = false;">RIGHT ▶</button>
        </div>

        <!-- Basket -->
        <div id="basket"></div>
    </div>

    <script>
        // Game variables
        const gameContainer = document.getElementById('gameContainer');
        const basket = document.getElementById('basket');
        const gameUI = document.getElementById('gameUI');
        const mobileControls = document.getElementById('mobileControls');
        const scoreDisplay = document.getElementById('score');
        const livesDisplay = document.getElementById('lives');
        const startScreen = document.getElementById('startScreen');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const finalScoreDisplay = document.getElementById('finalScore');

        let gameRunning = false;
        let score = 0;
        let lives = 3;
        let gameTime = 0;

        // Basket control
        let basketX = window.innerWidth / 2 - 30;
        let moveLeft = false;
        let moveRight = false;
        const basketSpeed = 8;
        const basketWidth = 60;

        // Falling objects
        let modaks = [];
        let gameLoopId = null;

        // Mobile detection
        const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);

        // Keyboard controls
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') moveLeft = true;
            if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') moveRight = true;
        });

        document.addEventListener('keyup', (e) => {
            if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') moveLeft = false;
            if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') moveRight = false;
        });

        // Touch controls
        gameContainer.addEventListener('touchmove', (e) => {
            if (!gameRunning) return;
            const touch = e.touches[0];
            const containerRect = gameContainer.getBoundingClientRect();
            const touchX = touch.clientX - containerRect.left;
            const basketCenter = basketX + basketWidth / 2;
            
            if (touchX < basketCenter - 20) {
                moveLeft = true;
                moveRight = false;
            } else if (touchX > basketCenter + 20) {
                moveRight = true;
                moveLeft = false;
            }
        }, { passive: true });

        gameContainer.addEventListener('touchend', () => {
            moveLeft = false;
            moveRight = false;
        });

        function startGame() {
            score = 0;
            lives = 3;
            gameTime = 0;
            modaks = [];
            basketX = window.innerWidth / 2 - 30;
            moveLeft = false;
            moveRight = false;

            gameRunning = true;
            gameUI.style.display = 'flex';
            startScreen.classList.remove('active');
            gameOverScreen.classList.remove('active');
            
            if (isMobile) {
                mobileControls.classList.add('active');
            }

            updateUI();
            gameLoop();
        }

        function endGame() {
            gameRunning = false;
            gameUI.style.display = 'none';
            mobileControls.classList.remove('active');
            finalScoreDisplay.textContent = score;
            gameOverScreen.classList.add('active');
            clearGameLoop();
            modaks.forEach(m => m.element.remove());
            modaks = [];
        }

        function updateUI() {
            scoreDisplay.textContent = score;
            livesDisplay.textContent = lives;
        }

        function createModak() {
            const isObstacle = Math.random() < 0.15; // 15% chance of obstacle
            const x = Math.random() * (window.innerWidth - 40);
            const element = document.createElement('div');
            element.className = isObstacle ? 'obstacle' : 'modak';
            element.style.left = x + 'px';
            element.style.top = '-40px';
            gameContainer.appendChild(element);

            modaks.push({
                element,
                x,
                y: -40,
                isObstacle,
                width: isObstacle ? 35 : 30,
                height: isObstacle ? 35 : 30
            });
        }

        function updateBasket() {
            if (moveLeft && basketX > 0) {
                basketX -= basketSpeed;
            }
            if (moveRight && basketX < window.innerWidth - basketWidth) {
                basketX += basketSpeed;
            }
            basket.style.left = basketX + 'px';
        }

        function updateModaks() {
            const basketY = window.innerHeight - 70;
            const basketRect = {
                x: basketX,
                y: basketY,
                width: basketWidth,
                height: 50
            };

            for (let i = modaks.length - 1; i >= 0; i--) {
                const modak = modaks[i];
                modak.y += 5 + gameTime * 0.05; // Speed increases over time
                modak.element.style.top = modak.y + 'px';

                // Check collision with basket
                if (
                    modak.x < basketRect.x + basketRect.width &&
                    modak.x + modak.width > basketRect.x &&
                    modak.y < basketRect.y + basketRect.height &&
                    modak.y + modak.height > basketRect.y
                ) {
                    modak.element.remove();
                    modaks.splice(i, 1);

                    if (modak.isObstacle) {
                        lives--;
                        createFloatingText('−1 Life!', modak.x, modak.y, '#FF6B6B');
                        if (lives <= 0) endGame();
                    } else {
                        score += 10;
                        createFloatingText('+10', modak.x, modak.y, '#FFD700');
                    }
                    updateUI();
                } else if (modak.y > window.innerHeight) {
                    modak.element.remove();
                    modaks.splice(i, 1);

                    if (!modak.isObstacle) {
                        lives--;
                        if (lives <= 0) endGame();
                        updateUI();
                    }
                }
            }
        }

        function createFloatingText(text, x, y, color) {
            const floatingText = document.createElement('div');
            floatingText.className = 'floatingText';
            floatingText.textContent = text;
            floatingText.style.left = x + 'px';
            floatingText.style.top = y + 'px';
            floatingText.style.color = color;
            gameContainer.appendChild(floatingText);

            setTimeout(() => floatingText.remove(), 1000);
        }

        function gameLoop() {
            if (!gameRunning) return;

            gameTime++;
            updateBasket();
            updateModaks();

            // Spawn modaks based on difficulty
            const spawnRate = Math.max(20, 60 - gameTime / 50);
            if (gameTime % Math.floor(spawnRate) === 0) {
                createModak();
            }

            gameLoopId = requestAnimationFrame(gameLoop);
        }

        function clearGameLoop() {
            if (gameLoopId) cancelAnimationFrame(gameLoopId);
        }

        // Handle window resize
        window.addEventListener('resize', () => {
            if (basketX + basketWidth > window.innerWidth) {
                basketX = window.innerWidth - basketWidth;
            }
        });
    </script>
</body>
</html>
