<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Emprendedor</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background: #1a202c; color: white; font-family: 'Segoe UI', sans-serif; }
        canvas { background: #2d3748; border: 4px solid #ecc94b; border-radius: 8px; box-shadow: 0 0 20px rgba(236, 201, 75, 0.3); image-rendering: pixelated; }
    </style>
</head>
<body class="flex flex-col items-center justify-center min-h-screen p-2">

    <div class="text-center mb-4">
        <h1 class="text-4xl font-black text-yellow-400 mb-2">🐍 Snake Emprendedor</h1>
        <p class="text-blue-300">¡Recoge ideas (💡) y pivota ante los rechazos (❌)!</p>
        <div class="mt-4 flex gap-6 justify-center font-bold text-2xl">
            <div class="bg-blue-900 px-4 py-1 rounded-lg">XP: <span id="xp">0</span></div>
            <div class="bg-green-900 px-4 py-1 rounded-lg">Nivel: <span id="nivel">1</span></div>
        </div>
    </div>

    <canvas id="gameCanvas" width="400" height="400"></canvas>
    
    <div id="msg" class="mt-4 text-red-400 font-bold hidden">¡Recibiste un "NO"! ¡Ajusta tu idea y sigue adelante!</div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        let xp = 0, nivel = 1, snake = [{x: 10, y: 10}], food = {x: 15, y: 15}, dx = 1, dy = 0;

        function draw() {
            ctx.clearRect(0, 0, 400, 400);
            ctx.fillStyle = "#48bb78";
            snake.forEach(p => ctx.fillRect(p.x * 20, p.y * 20, 18, 18));
            ctx.fillStyle = "#ecc94b";
            ctx.font = "20px Arial";
            ctx.fillText("💡", food.x * 20, food.y * 20 + 15);

            let head = {x: snake[0].x + dx, y: snake[0].y + dy};
            snake.unshift(head);
            if (head.x === food.x && head.y === food.y) {
                xp += 10; document.getElementById('xp').innerText = xp;
                food = {x: Math.floor(Math.random()*20), y: Math.floor(Math.random()*20)};
                if(xp % 50 === 0) nivel++;
                document.getElementById('nivel').innerText = nivel;
            } else { snake.pop(); }

            if(head.x < 0 || head.x >= 20 || head.y < 0 || head.y >= 20) {
                document.getElementById('msg').classList.remove('hidden');
                setTimeout(() => document.getElementById('msg').classList.add('hidden'), 2000);
                snake = [{x: 10, y: 10}];
            }
            setTimeout(draw, 150 - (nivel * 10));
        }

        window.addEventListener('keydown', e => {
            if(e.key === 'ArrowUp' && dy !== 1) { dx=0; dy=-1; }
            else if(e.key === 'ArrowDown' && dy !== -1) { dx=0; dy=1; }
            else if(e.key === 'ArrowLeft' && dx !== 1) { dx=-1; dy=0; }
            else if(e.key === 'ArrowRight' && dx !== -1) { dx=1; dy=0; }
        });
        draw();
    </script>
</body>
</html>
