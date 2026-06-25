<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Valle - Game Mobil</title>
    <style>
        :root {
            --safe-bottom: env(safe-area-inset-bottom, 20px);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-user-select: none;
            user-select: none;
            -webkit-touch-callout: none;
        }

        body {
            background: #1a1a2e;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            height: 100dvh;
            overflow: hidden;
            font-family: 'Segoe UI', 'Arial', sans-serif;
            touch-action: manipulation;
            margin: 0;
            padding: 0;
        }

        .game-wrapper {
            position: relative;
            width: 100vw;
            max-width: 480px;
            height: 100vh;
            height: 100dvh;
            max-height: 850px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: linear-gradient(180deg, #0f0f1a 0%, #1a1a2e 30%, #16213e 100%);
            border-radius: 0;
            overflow: hidden;
            box-shadow: 0 0 60px rgba(0, 180, 255, 0.15);
        }

        @media (min-width: 481px) {
            .game-wrapper {
                border-radius: 24px;
                margin: 10px;
                height: 90vh;
                height: 90dvh;
            }
        }

        .game-header {
            width: 100%;
            padding: 12px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 10;
            flex-shrink: 0;
        }

        .game-title {
            font-size: 1.6rem;
            font-weight: 900;
            color: #fff;
            letter-spacing: 2px;
            text-shadow: 0 0 20px rgba(255, 100, 50, 0.8), 0 0 40px rgba(255, 150, 50, 0.5);
            background: linear-gradient(135deg, #ff6b35, #ff4500);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .game-title span {
            font-size: 0.7rem;
            -webkit-text-fill-color: #ffaa00;
            letter-spacing: 1px;
        }

        .score-display {
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            padding: 8px 18px;
            border-radius: 25px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            color: #fff;
            font-weight: 700;
            font-size: 1.1rem;
            letter-spacing: 1px;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .score-display .icon {
            font-size: 1.3rem;
        }
        .score-value {
            color: #ffdd57;
            font-size: 1.3rem;
            min-width: 40px;
            text-align: center;
            transition: transform 0.1s ease;
        }
        .score-pop {
            transform: scale(1.3);
            color: #ff6b35;
        }

        canvas {
            display: block;
            flex: 1;
            width: 100%;
            cursor: pointer;
            border-top: 2px solid rgba(255, 255, 255, 0.1);
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }

        .controls {
            width: 100%;
            padding: 10px 15px calc(10px + var(--safe-bottom));
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 20px;
            flex-shrink: 0;
            z-index: 10;
        }

        .btn-dir {
            flex: 1;
            max-width: 150px;
            padding: 22px 10px;
            font-size: 2.5rem;
            font-weight: 700;
            border: none;
            border-radius: 20px;
            cursor: pointer;
            color: #fff;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            border: 2px solid rgba(255, 255, 255, 0.25);
            transition: all 0.15s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            letter-spacing: 2px;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
            position: relative;
            overflow: hidden;
        }
        .btn-dir:active {
            background: rgba(255, 255, 255, 0.25);
            border-color: rgba(255, 255, 255, 0.6);
            transform: scale(0.93);
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.3);
        }
        .btn-dir::after {
            content: '';
            position: absolute;
            inset: 0;
            background: radial-gradient(circle at center, rgba(255, 255, 255, 0.3) 0%, transparent 70%);
            opacity: 0;
            transition: opacity 0.15s;
        }
        .btn-dir:active::after {
            opacity: 1;
        }
        .btn-left {
            border-radius: 30px 20px 20px 30px;
        }
        .btn-right {
            border-radius: 20px 30px 30px 20px;
        }

        .overlay {
            position: absolute;
            inset: 0;
            background: rgba(0, 0, 0, 0.75);
            backdrop-filter: blur(6px);
            -webkit-backdrop-filter: blur(6px);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 20;
            gap: 16px;
            transition: opacity 0.3s ease;
            pointer-events: all;
        }
        .overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .overlay-title {
            font-size: 2.5rem;
            font-weight: 900;
            color: #fff;
            letter-spacing: 3px;
            text-shadow: 0 0 30px #ff4500, 0 0 60px #ff6b35;
            animation: pulse 1.5s ease-in-out infinite;
        }
        @keyframes pulse {
            0%,
            100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.06);
            }
        }
        .overlay-subtitle {
            color: #ccc;
            font-size: 0.95rem;
            letter-spacing: 1px;
            text-align: center;
            line-height: 1.5;
        }
        .btn-play {
            padding: 16px 50px;
            font-size: 1.3rem;
            font-weight: 700;
            letter-spacing: 2px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            color: #fff;
            background: linear-gradient(135deg, #ff4500, #ff6b35);
            box-shadow: 0 8px 30px rgba(255, 69, 0, 0.5);
            transition: all 0.2s ease;
            touch-action: manipulation;
        }
        .btn-play:active {
            transform: scale(0.9);
            box-shadow: 0 4px 15px rgba(255, 69, 0, 0.7);
        }
        .overlay-score {
            font-size: 1.8rem;
            color: #ffdd57;
            font-weight: 700;
            letter-spacing: 2px;
        }
        .best-score {
            color: #aaa;
            font-size: 0.9rem;
            letter-spacing: 1px;
        }
        .best-score span {
            color: #ffdd57;
            font-weight: 700;
        }
    </style>
</head>
<body>
    <div class="game-wrapper" id="gameWrapper">
        <!-- Header -->
        <div class="game-header">
            <div class="game-title">VALLE<span>🚗💨</span></div>
            <div class="score-display">
                <span class="icon">🏆</span>
                <span class="score-value" id="scoreDisplay">0</span>
            </div>
        </div>

        <!-- Canvas game -->
        <canvas id="gameCanvas"></canvas>

        <!-- Kontrol sentuh -->
        <div class="controls">
            <button class="btn-dir btn-left" id="btnLeft" aria-label="Geser kiri">⬅️</button>
            <button class="btn-dir btn-right" id="btnRight" aria-label="Geser kanan">➡️</button>
        </div>

        <!-- Overlay Mulai -->
        <div class="overlay" id="overlayStart">
            <div class="overlay-title">VALLE</div>
            <div class="overlay-subtitle">
                Hindari mobil lain di jalan!<br>Gunakan tombol ⬅️ ➡️ atau geser layar
            </div>
            <button class="btn-play" id="btnPlay">▶ MAIN</button>
            <div class="best-score" id="bestScoreStart">🏅 Terbaik: <span>0</span></div>
        </div>

        <!-- Overlay Game Over -->
        <div class="overlay hidden" id="overlayGameOver">
            <div class="overlay-title" style="font-size:2rem;">💥 GAME OVER</div>
            <div class="overlay-score" id="finalScore">Skor: 0</div>
            <button class="btn-play" id="btnRestart">🔄 MAIN LAGI</button>
            <div class="best-score" id="bestScoreOver">🏅 Terbaik: <span>0</span></div>
        </div>
    </div>

    <script>
        (function() {
            // ============ ELEMEN DOM ============
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const scoreDisplay = document.getElementById('scoreDisplay');
            const overlayStart = document.getElementById('overlayStart');
            const overlayGameOver = document.getElementById('overlayGameOver');
            const finalScoreEl = document.getElementById('finalScore');
            const bestScoreStart = document.getElementById('bestScoreStart');
            const bestScoreOver = document.getElementById('bestScoreOver');
            const btnLeft = document.getElementById('btnLeft');
            const btnRight = document.getElementById('btnRight');
            const btnPlay = document.getElementById('btnPlay');
            const btnRestart = document.getElementById('btnRestart');
            const gameWrapper = document.getElementById('gameWrapper');

            // ============ VARIABEL GAME ============
            let gameRunning = false;
            let gameOver = false;
            let score = 0;
            let bestScore = 0;
            let roadSpeed = 2.5;
            let baseSpeed = 2.5;
            let maxSpeed = 10;
            let speedIncrement = 0.003;
            let frameCount = 0;

            // Dimensi canvas
            let canvasWidth, canvasHeight;

            // Mobil pemain
            const playerCar = {
                x: 0,
                y: 0,
                width: 50,
                height: 85,
                color: '#ff4500',
                accentColor: '#ff6b35',
                name: 'VALLE',
            };

            // Lane (jalur)
            const laneCount = 3;
            let laneWidth = 0;
            let roadLeft = 0;
            let roadRight = 0;
            let currentLane = 1; // 0=kiri, 1=tengah, 2=kanan
            let targetLaneX = 0;

            // Marka jalan
            let roadMarkOffset = 0;

            // Mobil musuh
            let enemies = [];
            const enemyColors = [
                '#3498db', '#2ecc71', '#f39c12', '#9b59b6',
                '#1abc9c', '#e74c3c', '#00bcd4', '#ff9800',
                '#8bc34a', '#e91e63',
            ];
            let enemySpawnTimer = 0;
            let enemySpawnInterval = 70; // frame
            let minEnemyInterval = 30;

            // Partikel (untuk efek tabrakan / knalpot)
            let particles = [];

            // Input state
            let inputLeft = false;
            let inputRight = false;
            let touchActiveLeft = false;
            let touchActiveRight = false;

            // Animasi skor
            let scorePopTimer = 0;

            // ============ BEST SCORE (localStorage) ============
            try {
                const saved = localStorage.getItem('valle_best_score');
                if (saved !== null) bestScore = parseInt(saved, 10) || 0;
            } catch (e) {
                bestScore = 0;
            }

            function saveBestScore() {
                try {
                    localStorage.setItem('valle_best_score', bestScore.toString());
                } catch (e) {}
            }

            function updateBestScoreDisplay() {
                const spanStart = bestScoreStart.querySelector('span');
                const spanOver = bestScoreOver.querySelector('span');
                if (spanStart) spanStart.textContent = bestScore;
                if (spanOver) spanOver.textContent = bestScore;
            }
            updateBestScoreDisplay();

            // ============ RESIZE CANVAS ============
            function resizeCanvas() {
                const rect = gameWrapper.getBoundingClientRect();
                const headerHeight = 56;
                const controlsHeight = 110;
                const availableHeight = rect.height - headerHeight - controlsHeight;
                canvasWidth = Math.min(rect.width, 480);
                canvasHeight = Math.max(availableHeight, 300);

                canvas.width = canvasWidth;
                canvas.height = canvasHeight;
                canvas.style.width = canvasWidth + 'px';
                canvas.style.height = canvasHeight + 'px';

                // Hitung ulang lane
                const roadWidth = canvasWidth * 0.85;
                roadLeft = (canvasWidth - roadWidth) / 2;
                roadRight = roadLeft + roadWidth;
                laneWidth = roadWidth / laneCount;

                // Update posisi mobil pemain
                playerCar.width = Math.max(38, canvasWidth * 0.11);
                playerCar.height = playerCar.width * 1.7;
                playerCar.y = canvasHeight - playerCar.height - 20;
                targetLaneX = roadLeft + currentLane * laneWidth + laneWidth / 2 - playerCar.width / 2;
                playerCar.x = targetLaneX;

                // Update posisi musuh berdasarkan lane baru
                enemies.forEach(enemy => {
                    enemy.width = playerCar.width * 0.95;
                    enemy.height = playerCar.height * 0.95;
                    enemy.x = roadLeft + enemy.lane * laneWidth + laneWidth / 2 - enemy.width / 2;
                });
            }

            window.addEventListener('resize', () => {
                resizeCanvas();
                if (!gameRunning && !gameOver) {
                    drawStaticRoad();
                }
            });

            // ============ GAME FUNCTIONS ============
            function spawnEnemy() {
                // Pilih lane acak
                const lane = Math.floor(Math.random() * laneCount);
                const enemyWidth = playerCar.width * 0.95;
                const enemyHeight = playerCar.height * 0.95;
                const x = roadLeft + lane * laneWidth + laneWidth / 2 - enemyWidth / 2;
                const y = -enemyHeight - Math.random() * 150;
                const color = enemyColors[Math.floor(Math.random() * enemyColors.length)];

                // Cek overlap dengan musuh lain yang baru spawn (hindari tumpukan)
                const tooClose = enemies.some(e => {
                    if (e.lane === lane && e.y < -enemyHeight + 60 && e.y > -enemyHeight - 100) {
                        return true;
                    }
                    return false;
                });

                if (!tooClose) {
                    enemies.push({
                        x: x,
                        y: y,
                        width: enemyWidth,
                        height: enemyHeight,
                        lane: lane,
                        color: color,
                        speedVariation: 0.8 + Math.random() * 0.5,
                    });
                }
            }

            function createExplosion(x, y, color) {
                for (let i = 0; i < 25; i++) {
                    const angle = Math.random() * Math.PI * 2;
                    const speed = 2 + Math.random() * 8;
                    particles.push({
                        x: x,
                        y: y,
                        vx: Math.cos(angle) * speed,
                        vy: Math.sin(angle) * speed,
                        life: 1,
                        decay: 0.02 + Math.random() * 0.05,
                        size: 2 + Math.random() * 5,
                        color: color,
                    });
                }
            }

            function createExhaustParticle() {
                if (!gameRunning) return;
                const x = playerCar.x + playerCar.width / 2 + (Math.random() - 0.5) * 10;
                const y = playerCar.y + playerCar.height;
                particles.push({
                    x: x,
                    y: y,
                    vx: (Math.random() - 0.5) * 0.8,
                    vy: 1.5 + Math.random() * 3,
                    life: 1,
                    decay: 0.04 + Math.random() * 0.06,
                    size: 1.5 + Math.random() * 3,
                    color: 'rgba(200,200,200,0.8)',
                });
            }

            function resetGame() {
                score = 0;
                roadSpeed = baseSpeed;
                enemies = [];
                particles = [];
                enemySpawnTimer = 0;
                enemySpawnInterval = 70;
                frameCount = 0;
                currentLane = 1;
                roadMarkOffset = 0;
                scorePopTimer = 0;
                inputLeft = false;
                inputRight = false;
                touchActiveLeft = false;
                touchActiveRight = false;
                gameOver = false;
                updateScoreDisplay();
                resizeCanvas();
                playerCar.x = targetLaneX;
            }

            function startGame() {
                resetGame();
                gameRunning = true;
                gameOver = false;
                overlayStart.classList.add('hidden');
                overlayGameOver.classList.add('hidden');
                resizeCanvas();
                requestAnimationFrame(gameLoop);
            }

            function endGame() {
                gameRunning = false;
                gameOver = true;
                // Efek ledakan
                createExplosion(
                    playerCar.x + playerCar.width / 2,
                    playerCar.y + playerCar.height / 2,
                    playerCar.color
                );
                // Update best score
                if (score > bestScore) {
                    bestScore = score;
                    saveBestScore();
                }
                updateBestScoreDisplay();
                finalScoreEl.textContent = 'Skor: ' + score;
                overlayGameOver.classList.remove('hidden');
                updateScoreDisplay();
                // Render ledakan
                renderFrame();
            }

            function updateScoreDisplay() {
                scoreDisplay.textContent = score;
                if (scorePopTimer > 0) {
                    scoreDisplay.classList.add('score-pop');
                } else {
                    scoreDisplay.classList.remove('score-pop');
                }
            }

            // ============ COLLISION DETECTION ============
            function checkCollision(px, py, pw, ph, ex, ey, ew, eh) {
                // Sedikit缩小 hitbox untuk fairness
                const margin = 6;
                return (
                    px + margin < ex + ew - margin &&
                    px + pw - margin > ex + margin &&
                    py + margin < ey + eh - margin &&
                    py + ph - margin > ey + margin
                );
            }

            // ============ UPDATE ============
            function update() {
                if (!gameRunning || gameOver) return;

                frameCount++;

                // Update skor
                if (frameCount % 6 === 0) {
                    score++;
                    if (score % 50 === 0 && score > 0) {
                        scorePopTimer = 15;
                    }
                }
                if (scorePopTimer > 0) {
                    scorePopTimer--;
                }
                updateScoreDisplay();

                // Tingkatkan kecepatan
                roadSpeed = Math.min(maxSpeed, baseSpeed + score * speedIncrement);
                enemySpawnInterval = Math.max(minEnemyInterval, 70 - score * 0.15);

                // Update marka jalan
                roadMarkOffset = (roadMarkOffset + roadSpeed) % 50;

                // Update posisi target player berdasarkan input
                if (inputLeft && currentLane > 0) {
                    currentLane--;
                    inputLeft = false;
                }
                if (inputRight && currentLane < laneCount - 1) {
                    currentLane++;
                    inputRight = false;
                }
                targetLaneX = roadLeft + currentLane * laneWidth + laneWidth / 2 - playerCar.width / 2;

                // Smooth movement ke target lane
                const lerpSpeed = 0.22;
                playerCar.x += (targetLaneX - playerCar.x) * lerpSpeed;

                // Clamp posisi player
                playerCar.x = Math.max(roadLeft + 2, Math.min(roadRight - playerCar.width - 2, playerCar.x));

                // Spawn enemy
                enemySpawnTimer++;
                if (enemySpawnTimer >= enemySpawnInterval) {
                    enemySpawnTimer = 0;
                    if (enemies.length < 6) {
                        spawnEnemy();
                    }
                    // Kadang spawn 2 sekaligus
                    if (Math.random() < 0.2 && enemies.length < 5 && score > 30) {
                        spawnEnemy();
                    }
                }

                // Update enemy
                for (let i = enemies.length - 1; i >= 0; i--) {
                    const enemy = enemies[i];
                    enemy.y += roadSpeed * enemy.speedVariation;

                    // Cek tabrakan
                    if (
                        checkCollision(
                            playerCar.x, playerCar.y, playerCar.width, playerCar.height,
                            enemy.x, enemy.y, enemy.width, enemy.height
                        )
                    ) {
                        endGame();
                        return;
                    }

                    // Hapus enemy yang keluar layar
                    if (enemy.y > canvasHeight + 50) {
                        enemies.splice(i, 1);
                    }
                }

                // Update partikel
                for (let i = particles.length - 1; i >= 0; i--) {
                    const p = particles[i];
                    p.x += p.vx;
                    p.y += p.vy;
                    p.life -= p.decay;
                    if (p.life <= 0) {
                        particles.splice(i, 1);
                    }
                }

                // Partikel knalpot
                if (frameCount % 3 === 0) {
                    createExhaustParticle();
                }
            }

            // ============ RENDER ============
            function drawRoad() {
                // Aspal
                const gradient = ctx.createLinearGradient(0, 0, 0, canvasHeight);
                gradient.addColorStop(0, '#2c3e50');
                gradient.addColorStop(0.5, '#34495e');
                gradient.addColorStop(1, '#2c3e50');
                ctx.fillStyle = gradient;
                ctx.fillRect(roadLeft, 0, roadRight - roadLeft, canvasHeight);

                // Tepi jalan
                ctx.strokeStyle = '#ffffff';
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.moveTo(roadLeft, 0);
                ctx.lineTo(roadLeft, canvasHeight);
                ctx.stroke();
                ctx.beginPath();
                ctx.moveTo(roadRight, 0);
                ctx.lineTo(roadRight, canvasHeight);
                ctx.stroke();

                // Garis merah-putih tepi
                ctx.lineWidth = 1.5;
                for (let y = -50 + (roadMarkOffset % 30); y < canvasHeight + 50; y += 30) {
                    ctx.strokeStyle = '#e74c3c';
                    ctx.beginPath();
                    ctx.moveTo(roadLeft + 4, y);
                    ctx.lineTo(roadLeft + 4, y + 15);
                    ctx.stroke();
                    ctx.strokeStyle = '#ffffff';
                    ctx.beginPath();
                    ctx.moveTo(roadLeft + 4, y + 15);
                    ctx.lineTo(roadLeft + 4, y + 30);
                    ctx.stroke();

                    ctx.strokeStyle = '#e74c3c';
                    ctx.beginPath();
                    ctx.moveTo(roadRight - 4, y);
                    ctx.lineTo(roadRight - 4, y + 15);
                    ctx.stroke();
                    ctx.strokeStyle = '#ffffff';
                    ctx.beginPath();
                    ctx.moveTo(roadRight - 4, y + 15);
                    ctx.lineTo(roadRight - 4, y + 30);
                    ctx.stroke();
                }

                // Marka putus-putus antar lane
                ctx.setLineDash([20, 20]);
                ctx.lineDashOffset = -roadMarkOffset;
                ctx.strokeStyle = 'rgba(255,255,255,0.7)';
                ctx.lineWidth = 2.5;
                for (let i = 1; i < laneCount; i++) {
                    const lx = roadLeft + i * laneWidth;
                    ctx.beginPath();
                    ctx.moveTo(lx, 0);
                    ctx.lineTo(lx, canvasHeight);
                    ctx.stroke();
                }
                ctx.setLineDash([]);
                ctx.lineDashOffset = 0;
            }

            function drawCar(x, y, w, h, color, accentColor, isPlayer, name) {
                ctx.save();
                // Bayangan
                ctx.shadowColor = 'rgba(0,0,0,0.4)';
                ctx.shadowBlur = 8;
                ctx.shadowOffsetX = 2;
                ctx.shadowOffsetY = 4;

                // Body mobil
                const bodyGradient = ctx.createLinearGradient(x, y, x, y + h);
                bodyGradient.addColorStop(0, accentColor || color);
                bodyGradient.addColorStop(0.5, color);
                bodyGradient.addColorStop(1, '#000000');
                ctx.fillStyle = bodyGradient;

                // Bentuk mobil dengan rounded rect
                const radius = w * 0.25;
                ctx.beginPath();
                ctx.moveTo(x + radius, y);
                ctx.lineTo(x + w - radius, y);
                ctx.quadraticCurveTo(x + w, y, x + w, y + radius);
                ctx.lineTo(x + w, y + h - radius);
                ctx.quadraticCurveTo(x + w, y + h, x + w - radius, y + h);
                ctx.lineTo(x + radius, y + h);
                ctx.quadraticCurveTo(x, y + h, x, y + h - radius);
                ctx.lineTo(x, y + radius);
                ctx.quadraticCurveTo(x, y, x + radius, y);
                ctx.closePath();
                ctx.fill();

                ctx.shadowColor = 'transparent';
                ctx.shadowBlur = 0;
                ctx.shadowOffsetX = 0;
                ctx.shadowOffsetY = 0;

                // Jendela depan (kaca)
                const glassY = y + h * 0.08;
                const glassH = h * 0.28;
                const glassMargin = w * 0.12;
                ctx.fillStyle = 'rgba(180,220,255,0.7)';
                ctx.beginPath();
                ctx.moveTo(x + glassMargin, glassY);
                ctx.lineTo(x + w - glassMargin, glassY);
                ctx.lineTo(x + w - glassMargin - 4, glassY + glassH);
                ctx.lineTo(x + glassMargin + 4, glassY + glassH);
                ctx.closePath();
                ctx.fill();
                ctx.strokeStyle = 'rgba(255,255,255,0.6)';
                ctx.lineWidth = 1;
                ctx.stroke();

                // Jendela belakang
                const rearGlassY = y + h * 0.42;
                const rearGlassH = h * 0.22;
                ctx.fillStyle = 'rgba(140,190,230,0.6)';
                ctx.beginPath();
                ctx.moveTo(x + glassMargin, rearGlassY);
                ctx.lineTo(x + w - glassMargin, rearGlassY);
                ctx.lineTo(x + w - glassMargin - 3, rearGlassY + rearGlassH);
                ctx.lineTo(x + glassMargin + 3, rearGlassY + rearGlassH);
                ctx.closePath();
                ctx.fill();
                ctx.strokeStyle = 'rgba(255,255,255,0.5)';
                ctx.stroke();

                // Lampu depan (putih)
                if (isPlayer) {
                    ctx.fillStyle = '#ffffff';
                    ctx.shadowColor = '#ffffaa';
                    ctx.shadowBlur = 8;
                    ctx.fillRect(x + 5, y + h - 8, 7, 6);
                    ctx.fillRect(x + w - 12, y + h - 8, 7, 6);
                    ctx.shadowColor = 'transparent';
                    ctx.shadowBlur = 0;
                } else {
                    // Lampu belakang musuh (merah)
                    ctx.fillStyle = '#ff3333';
                    ctx.shadowColor = '#ff0000';
                    ctx.shadowBlur = 6;
                    ctx.fillRect(x + 5, y + 3, 7, 5);
                    ctx.fillRect(x + w - 12, y + 3, 7, 5);
                    ctx.shadowColor = 'transparent';
                    ctx.shadowBlur = 0;
                }

                // Nama di body (untuk player)
                if (isPlayer && name) {
                    ctx.fillStyle = '#ffffff';
                    ctx.font = `bold ${Math.max(9, w * 0.2)}px 'Segoe UI', Arial, sans-serif`;
                    ctx.textAlign = 'center';
                    ctx.fillText(name, x + w / 2, y + h * 0.68);
                    // Garis racing
                    ctx.strokeStyle = 'rgba(255,255,255,0.5)';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.moveTo(x + w * 0.2, y + h * 0.6);
                    ctx.lineTo(x + w * 0.8, y + h * 0.6);
                    ctx.stroke();
                }

                // Roda
                const wheelW = w * 0.18;
                const wheelH = h * 0.1;
                ctx.fillStyle = '#111';
                // Roda kiri depan
                ctx.fillRect(x - wheelW * 0.4, y + h * 0.12, wheelW, wheelH);
                // Roda kanan depan
                ctx.fillRect(x + w - wheelW * 0.6, y + h * 0.12, wheelW, wheelH);
                // Roda kiri belakang
                ctx.fillRect(x - wheelW * 0.4, y + h * 0.78, wheelW, wheelH);
                // Roda kanan belakang
                ctx.fillRect(x + w - wheelW * 0.6, y + h * 0.78, wheelW, wheelH);

                ctx.restore();
            }

            function drawParticles() {
                for (const p of particles) {
                    ctx.fillStyle = p.color.replace('0.8', String(p.life * 0.8));
                    ctx.globalAlpha = p.life;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                    ctx.fill();
                }
                ctx.globalAlpha = 1;
            }

            function drawStaticRoad() {
                ctx.clearRect(0, 0, canvasWidth, canvasHeight);
                // Background luar jalan
                ctx.fillStyle = '#1a3a1a';
                ctx.fillRect(0, 0, canvasWidth, canvasHeight);
                // Rumput pattern
                ctx.fillStyle = '#2d5a27';
                for (let i = 0; i < canvasWidth; i += 30) {
                    for (let j = 0; j < canvasHeight; j += 30) {
                        if (Math.random() < 0.3) {
                            ctx.fillRect(i, j, 15, 15);
                        }
                    }
                }
                drawRoad();
                // Gambar mobil player diam
                if (playerCar.width > 0) {
                    drawCar(
                        playerCar.x, playerCar.y,
                        playerCar.width, playerCar.height,
                        playerCar.color, playerCar.accentColor,
                        true, playerCar.name
                    );
                }
            }

            function renderFrame() {
                ctx.clearRect(0, 0, canvasWidth, canvasHeight);

                // Background luar jalan (rumput)
                ctx.fillStyle = '#1a3a1a';
                ctx.fillRect(0, 0, canvasWidth, canvasHeight);
                ctx.fillStyle = '#2d5a27';
                for (let i = 0; i < canvasWidth; i += 30) {
                    for (let j = 0; j < canvasHeight; j += 30) {
                        if ((i + j * 7) % 90 < 30) {
                            ctx.fillRect(i, j, 15, 15);
                        }
                    }
                }

                // Jalan
                drawRoad();

                // Gambar musuh
                for (const enemy of enemies) {
                    drawCar(
                        enemy.x, enemy.y,
                        enemy.width, enemy.height,
                        enemy.color, null,
                        false, null
                    );
                }

                // Gambar player
                if (!gameOver) {
                    drawCar(
                        playerCar.x, playerCar.y,
                        playerCar.width, playerCar.height,
                        playerCar.color, playerCar.accentColor,
                        true, playerCar.name
                    );
                } else {
                    // Gambar puing-puing (player hancur)
                    ctx.fillStyle = playerCar.color;
                    ctx.globalAlpha = 0.4;
                    const cx = playerCar.x + playerCar.width / 2;
                    const cy = playerCar.y + playerCar.height / 2;
                    for (let i = 0; i < 8; i++) {
                        const angle = (i / 8) * Math.PI * 2;
                        const dist = 15 + Math.random() * 25;
                        ctx.fillRect(
                            cx + Math.cos(angle) * dist - 6,
                            cy + Math.sin(angle) * dist - 6,
                            12, 12
                        );
                    }
                    ctx.globalAlpha = 1;
                }

                // Partikel
                drawParticles();

                // Overlay game over kecil di canvas
                if (gameOver) {
                    ctx.fillStyle = 'rgba(0,0,0,0.5)';
                    ctx.fillRect(0, 0, canvasWidth, canvasHeight);
                    ctx.fillStyle = '#fff';
                    ctx.font = 'bold 28px "Segoe UI", Arial, sans-serif';
                    ctx.textAlign = 'center';
                    ctx.fillText('💥 GAME OVER', canvasWidth / 2, canvasHeight / 2 - 10);
                    ctx.fillStyle = '#ffdd57';
                    ctx.font = 'bold 20px "Segoe UI", Arial, sans-serif';
                    ctx.fillText('Skor: ' + score, canvasWidth / 2, canvasHeight / 2 + 30);
                }
            }

            function gameLoop() {
                if (!gameRunning && !gameOver) {
                    // Jika game tidak running, tetap render statis
                    drawStaticRoad();
                    return;
                }
                if (gameOver) {
                    // Update partikel ledakan saja
                    for (let i = particles.length - 1; i >= 0; i--) {
                        const p = particles[i];
                        p.x += p.vx;
                        p.y += p.vy;
                        p.life -= p.decay;
                        if (p.life <= 0) particles.splice(i, 1);
                    }
                    renderFrame();
                    if (particles.length > 0) {
                        requestAnimationFrame(() => {
                            if (gameOver) {
                                renderFrame();
                                // Lanjutkan animasi partikel
                                const animParticles = () => {
                                    if (!gameOver || particles.length === 0) return;
                                    for (let i = particles.length - 1; i >= 0; i--) {
                                        const p = particles[i];
                                        p.x += p.vx;
                                        p.y += p.vy;
                                        p.life -= p.decay;
                                        if (p.life <= 0) particles.splice(i, 1);
                                    }
                                    renderFrame();
                                    if (particles.length > 0) {
                                        requestAnimationFrame(animParticles);
                                    }
                                };
                                requestAnimationFrame(animParticles);
                            }
                        });
                    }
                    return;
                }

                update();
                renderFrame();
                requestAnimationFrame(gameLoop);
            }

            // ============ INPUT HANDLING ============
            // Keyboard
            document.addEventListener('keydown', (e) => {
                if (!gameRunning || gameOver) return;
                if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') {
                    e.preventDefault();
                    inputLeft = true;
                }
                if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') {
                    e.preventDefault();
                    inputRight = true;
                }
            });

            document.addEventListener('keyup', (e) => {
                if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') {
                    inputLeft = false;
                }
                if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') {
                    inputRight = false;
                }
            });

            // Touch buttons
            function addTouchListeners(btn, onDown, onUp) {
                btn.addEventListener('pointerdown', (e) => {
                    e.preventDefault();
                    onDown();
                });
                btn.addEventListener('pointerup', (e) => {
                    e.preventDefault();
                    onUp();
                });
                btn.addEventListener('pointerleave', (e) => {
                    onUp();
                });
                btn.addEventListener('pointercancel', (e) => {
                    onUp();
                });
                // Cegah event mouse setelah touch
                btn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    onDown();
                });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    onUp();
                });
            }

            addTouchListeners(btnLeft,
                () => {
                    touchActiveLeft = true;
                    if (gameRunning && !gameOver) inputLeft = true;
                },
                () => {
                    touchActiveLeft = false;
                    inputLeft = false;
                }
            );

            addTouchListeners(btnRight,
                () => {
                    touchActiveRight = true;
                    if (gameRunning && !gameOver) inputRight = true;
                },
                () => {
                    touchActiveRight = false;
                    inputRight = false;
                }
            );

            // Swipe pada canvas
            let touchStartX = 0;
            let touchStartY = 0;
            let swipeHandled = false;

            canvas.addEventListener('touchstart', (e) => {
                if (!gameRunning || gameOver) return;
                if (e.touches.length === 1) {
                    touchStartX = e.touches[0].clientX;
                    touchStartY = e.touches[0].clientY;
                    swipeHandled = false;
                }
            }, { passive: true });

            canvas.addEventListener('touchmove', (e) => {
                if (!gameRunning || gameOver || swipeHandled) return;
                if (e.touches.length === 1) {
                    const dx = e.touches[0].clientX - touchStartX;
                    const dy = e.touches[0].clientY - touchStartY;
                    const threshold = 30;
                    if (Math.abs(dx) > threshold && Math.abs(dx) > Math.abs(dy)) {
                        if (dx < 0 && currentLane > 0) {
                            inputLeft = true;
                            swipeHandled = true;
                        } else if (dx > 0 && currentLane < laneCount - 1) {
                            inputRight = true;
                            swipeHandled = true;
                        }
                        touchStartX = e.touches[0].clientX;
                    }
                }
            }, { passive: true });

            canvas.addEventListener('touchend', () => {
                inputLeft = false;
                inputRight = false;
                swipeHandled = false;
            });

            // Mouse click pada canvas (desktop alternatif)
            canvas.addEventListener('click', (e) => {
                if (!gameRunning || gameOver) return;
                const rect = canvas.getBoundingClientRect();
                const clickX = e.clientX - rect.left;
                const midX = canvasWidth / 2;
                if (clickX < midX && currentLane > 0) {
                    inputLeft = true;
                } else if (clickX >= midX && currentLane < laneCount - 1) {
                    inputRight = true;
                }
                setTimeout(() => {
                    inputLeft = false;
                    inputRight = false;
                }, 50);
            });

            // ============ BUTTON OVERLAY ============
            btnPlay.addEventListener('click', (e) => {
                e.preventDefault();
                startGame();
            });
            btnPlay.addEventListener('touchend', (e) => {
                e.preventDefault();
                startGame();
            });

            btnRestart.addEventListener('click', (e) => {
                e.preventDefault();
                startGame();
            });
            btnRestart.addEventListener('touchend', (e) => {
                e.preventDefault();
                startGame();
            });

            // ============ INIT ============
            function init() {
                resizeCanvas();
                updateBestScoreDisplay();
                overlayStart.classList.remove('hidden');
                overlayGameOver.classList.add('hidden');
                gameRunning = false;
                gameOver = false;
                updateScoreDisplay();
                drawStaticRoad();
            }

            init();

            // Re-render saat orientasi berubah
            window.addEventListener('orientationchange', () => {
                setTimeout(() => {
                    resizeCanvas();
                    if (!gameRunning && !gameOver) {
                        drawStaticRoad();
                    }
                }, 300);
            });

            console.log('🚗💨 Valle - Game Mobil siap dimainkan!');
            console.log('   Gunakan tombol ⬅️ ➡️ atau swipe layar');
            console.log('   Tekan MAIN untuk mulai bermain!');
        })();
    </script>
</body>
</html>
