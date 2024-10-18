<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pattern Memory Game</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
</head>
<body>
    <style>/* style.css */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }
        
        #game-container {
            display: grid;
            margin: 20px auto;
            width: fit-content;
            gap: 5px;
        }
        
        .grid-cell {
            width: 50px;
            height: 50px;
            background-color: #8b4513; /* Brown background */
            border: 1px solid #000;
        }
        
        .grid-cell.active {
            background-color: #00FFFF; /* Highlighted (cyan) */
        }
        
        button {
            padding: 10px 20px;
            margin-top: 20px;
            font-size: 16px;
        }
        </style>
    <h1>Pattern Memory Game</h1>
    <div id="game-container"></div>
    <button id="start-game">Start Game</button>

    <!-- <script src="script.js"></script>
      -->
    <script>// script.js

        // Game variables
        let gridSize = 3;  // Starting grid size
        let pattern = [];
        let userPattern = [];
        let allowInput = false; // Control when user can input
        
        const gameContainer = document.getElementById('game-container');
        const startButton = document.getElementById('start-game');
        
        // Function to create the grid
        function createGrid(size) {
            gameContainer.innerHTML = ''; // Clear the grid
            gameContainer.style.gridTemplateColumns = `repeat(${size}, 50px)`;
            
            for (let i = 0; i < size * size; i++) {
                const cell = document.createElement('div');
                cell.classList.add('grid-cell');
                cell.addEventListener('click', () => handleCellClick(i));
                gameContainer.appendChild(cell);
            }
        }
        
        // Function to generate a random pattern
        function generatePattern(size) {
            pattern = [];
            const numCells = size * size;
            const patternLength = Math.floor(size * 1.5); // Increase pattern as grid grows
            
            while (pattern.length < patternLength) {
                const randomIndex = Math.floor(Math.random() * numCells);
                if (!pattern.includes(randomIndex)) {
                    pattern.push(randomIndex);
                }
            }
        }
        
        // Function to display the pattern
        function showPattern() {
            allowInput = false;
            userPattern = [];
            
            pattern.forEach((index, i) => {
                setTimeout(() => {
                    const cell = document.querySelectorAll('.grid-cell')[index];
                    cell.classList.add('active');
                }, i * 500);
                
                setTimeout(() => {
                    const cell = document.querySelectorAll('.grid-cell')[index];
                    cell.classList.remove('active');
                    if (i === pattern.length - 1) {
                        allowInput = true;  // Allow user to start clicking
                    }
                }, (i + 1) * 500);
            });
        }
        
        // Handle user cell clicks
        function handleCellClick(index) {
            if (!allowInput) return;
        
            userPattern.push(index);
            const cell = document.querySelectorAll('.grid-cell')[index];
            cell.classList.add('active');
            
            // Check if user pattern matches
            if (userPattern.length === pattern.length) {
                checkPattern();
            }
        }
        
        // Check if user got the pattern right
        function checkPattern() {
            if (JSON.stringify(userPattern) === JSON.stringify(pattern)) {
                alert('Correct! Moving to next level.');
                gridSize++;
                startGame();  // Increase grid size and start next round
            } else {
                alert('Incorrect! Try again.');
                userPattern = [];
            }
        }
        
        // Start the game
        function startGame() {
            createGrid(gridSize);
            generatePattern(gridSize);
            setTimeout(showPattern, 1000); // Show pattern after grid is ready
        }
        
        // Event listener for the start button
        startButton.addEventListener('click', startGame);
        </script>
</body>
</html>
