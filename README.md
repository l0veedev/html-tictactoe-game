# HTML Tic-Tac-Toe Game

A lightweight, high-performance open-source Tic-Tac-Toe game built with pure HTML5 Canvas and JavaScript. Written in a single file with zero external dependencies or assets, featuring an AI opponent.

### How to Play
- Tap or click on any empty cell to place your cross (**X**).
- The AI plays as the red circle (**O**).
- Get three in a row horizontally, vertically, or diagonally to win!
- Tap the screen after the game ends to restart.

### Play Online
You can play the game instantly in your browser via [GitHub Pages](https://l0veedev.github.io/html-tictacttoe-game/).

---

### Source Code (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML Tic-Tac-Toe Game — Play Online | l0veedev</title>
    <meta name="description" content="A lightweight, high-performance open-source Tic-Tac-Toe game built with pure HTML5 Canvas and JavaScript. Play online against an AI!">
    <meta name="keywords" content="html tic tac toe, javascript cross zero, retro arcade, l0veedev">
    <meta name="author" content="l0veedev">
    <style>
        body {
            margin: 0;
            padding: 20px;
            background: #0d0d0d;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            box-sizing: border-box;
            outline: none;
            color: #fff;
            font-family: sans-serif;
            user-select: none;
        }
        h1 {
            margin: 0 0 15px 0;
            font-size: 22px;
            letter-spacing: 2px;
            color: #e0e0e0;
        }
        canvas {
            background: #1a1a1a;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.8);
            border-radius: 4px;
            cursor: pointer;
            margin-bottom: 20px;
            touch-action: none;
        }
        .comments-container {
            width: 100%;
            max-width: 360px;
        }
    </style>
</head>
<body>
    <h1>TIC-TAC-TOE</h1>
    <canvas width="360" height="360" id="game"></canvas>

    <div class="comments-container">
        <script src="[https://giscus.app/client.js](https://giscus.app/client.js)"
                data-repo="l0veedev/html-snake-game"
                data-repo-id="R_kgDOVnXJAA"
                data-category="Announcements"
                data-category-id="DIC_kwDOVnXJAM4DKrfR"
                data-mapping="pathname"
                data-strict="0"
                data-reactions-enabled="1"
                data-emit-metadata="0"
                data-input-position="bottom"
                data-theme="dark"
                data-lang="ru"
                crossorigin="anonymous"
                async>
        </script>
    </div>

    <script>
        const canvas = document.getElementById('game');
        const ctx = canvas.getContext('2d');
        const size = 360;
        const cell = size / 3;

        let grid = ['', '', '', '', '', '', '', '', ''];
        let active = true;
        let isBotThinking = false;
        let text = '';

        function render() {
            ctx.fillStyle = '#1a1a1a';
            ctx.fillRect(0, 0, size, size);

            ctx.strokeStyle = '#333';
            ctx.lineWidth = 4;
            ctx.lineCap = 'round';

            for (let i = 1; i < 3; i++) {
                ctx.beginPath();
                ctx.moveTo(i * cell, 20);
                ctx.lineTo(i * cell, size - 20);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(20, i * cell);
                ctx.lineTo(size - 20, i * cell);
                ctx.stroke();
            }

            grid.forEach((val, i) => {
                let x = (i % 3) * cell + cell / 2;
                let y = Math.floor(i / 3) * cell + cell / 2;

                if (val === 'X') {
                    ctx.strokeStyle = '#4CAF50';
                    ctx.lineWidth = 6;
                    ctx.beginPath();
                    ctx.moveTo(x - 35, y - 35);
                    ctx.lineTo(x + 35, y + 35);
                    ctx.moveTo(x + 35, y - 35);
                    ctx.lineTo(x - 35, y + 35);
                    ctx.stroke();
                } else if (val === 'O') {
                    ctx.strokeStyle = '#e53935';
                    ctx.lineWidth = 6;
                    ctx.beginPath();
                    ctx.arc(x, y, 35, 0, Math.PI * 2);
                    ctx.stroke();
                }
            });

            if (!active) {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.8)';
                ctx.fillRect(0, 0, size, size);

                ctx.fillStyle = '#fff';
                ctx.font = '28px sans-serif';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(text, size / 2, size / 2 - 15);

                ctx.font = '14px sans-serif';
                ctx.fillStyle = '#aaa';
                ctx.fillText('Tap to restart', size / 2, size / 2 + 30);
            }
        }

        function checkWinner(board) {
            const wins = [
                [0,1,2], [3,4,5], [6,7,8],
                [0,3,6], [1,4,7], [2,5,8],
                [0,4,8], [2,4,6]
            ];
            for (let w of wins) {
                let [a, b, c] = w;
                if (board[a] && board[a] === board[b] && board[a] === board[c]) {
                    return board[a];
                }
