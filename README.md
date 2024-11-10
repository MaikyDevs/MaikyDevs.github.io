
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe - Lokales Spiel</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: linear-gradient(45deg, #ff9a9e, #fad0c4, #fad0c4, #a18cd1);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 800px;
            padding: 20px;
            text-align: center;
        }

        h1 {
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 30px;
        }

        .game-status {
            font-size: 1.4em;
            color: white;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
            background: rgba(0,0,0,0.2);
            padding: 10px 20px;
            border-radius: 20px;
            margin: 10px 0;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 20px auto;
            max-width: 400px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 15px;
        }

        .cell {
            width: 100px;
            height: 100px;
            border: none;
            border-radius: 10px;
            font-size: 2.5em;
            cursor: pointer;
            background: rgba(255, 255, 255, 0.9);
            transition: all 0.3s;
        }

        .cell:hover {
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(255,255,255,0.5);
        }

        .cell.x {
            color: #FF69B4;
        }

        .cell.o {
            color: #4169E1;
        }

        .button {
            background: linear-gradient(45deg, #ff6b6b, #ff8e8e);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            margin: 10px;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.3);
        }

        .winning-cell {
            animation: winner 1s infinite;
        }

        @keyframes winner {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎮 Tic Tac Toe 🎮</h1>
        
        <div id="gameStatus" class="game-status">Spieler X ist am Zug</div>
        
        <div class="game-board" id="board">
            <button class="cell" data-index="0"></button>
            <button class="cell" data-index="1"></button>
            <button class="cell" data-index="2"></button>
            <button class="cell" data-index="3"></button>
            <button class="cell" data-index="4"></button>
            <button class="cell" data-index="5"></button>
            <button class="cell" data-index="6"></button>
            <button class="cell" data-index="7"></button>
            <button class="cell" data-index="8"></button>
        </div>

        <button onclick="resetGame()" class="button">Neues Spiel</button>
    </div>

    <script>
        let currentPlayer = 'X';
        let gameBoard = ['', '', '', '', '', '', '', '', ''];
        let gameActive = true;

        const winningCombinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Reihen
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Spalten
            [0, 4, 8], [2, 4, 6] // Diagonalen
        ];

        const cells = document.querySelectorAll('.cell');
        const statusDisplay = document.getElementById('gameStatus');

        cells.forEach(cell => {
            cell.addEventListener('click', handleCellClick);
        });

        function handleCellClick(event) {
            const cell = event.target;
            const index = cell.getAttribute('data-index');

            if (gameBoard[index] !== '' || !gameActive) {
                return;
            }

            gameBoard[index] = currentPlayer;
            cell.textContent = currentPlayer;
            cell.classList.add(currentPlayer.toLowerCase());

            if (checkWin()) {
                gameActive = false;
                statusDisplay.textContent = Spieler ${currentPlayer} gewinnt! 🎉;
                highlightWinningCombination();
                return;
            }

            if (checkDraw()) {
                gameActive = false;
                statusDisplay.textContent = 'Unentschieden! 🤝';
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            statusDisplay.textContent = Spieler ${currentPlayer} ist am Zug;
        }

        function checkWin() {
            return winningCombinations.some(combination => {
                return combination.every(index => {
                    return gameBoard[index] === currentPlayer;
                });
            });
        }

        function checkDraw() {
            return gameBoard.every(cell => cell !== '');
        }

        function highlightWinningCombination() {
            winningCombinations.forEach(combination => {
                if (combination.every(index => gameBoard[index] === currentPlayer)) {
                    combination.forEach(index => {
                        cells[index].classList.add('winning-cell');
                    });
                }
            });
        }

        function resetGame() {
            currentPlayer = 'X';
            gameBoard = ['', '', '', '', '', '', '', '', ''];
            gameActive = true;
            statusDisplay.textContent = Spieler ${currentPlayer} ist am Zug;
            
            cells.forEach(cell => {
                cell.textContent = '';
                cell.classList.remove('x', 'o', 'winning-cell');
            });
        }
    </script>
</body>
</html>