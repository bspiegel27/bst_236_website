<div style="text-align: right; margin: 10px;">
    <a href="https://bspiegel27.github.io/bst_236_website/" style="text-decoration: none; color: #0366d6; font-weight: bold;">← Home</a>
</div>
# Pacman Game Implementation

## Overview
This is a simple implementation of the classic Pacman game using HTML, CSS, and JavaScript. The game features:
- Pacman character controlled by arrow keys
- Collectable pellets for scoring
- A ghost that follows Pacman
- Score tracking
- Game over when caught by the ghost

## How to Play
1. Use the arrow keys to move Pacman
2. Collect white pellets to increase your score
3. Avoid the red ghost
4. Try to get the highest score possible

## Play the Game
<div class="score">Score: <span id="score">0</span></div>
<div id="game-board" class="game-board">
    <div id="pacman" class="pacman"></div>
</div>
<style>
    .game-board {
        width: 500px;
        height: 500px;
        border: 2px solid blue;
        position: relative;
        background: black;
        margin: 0 auto;
    }
    .pacman {
        width: 30px;
        height: 30px;
        background: yellow;
        border-radius: 50%;
        position: absolute;
        clip-path: polygon(100% 0, 100% 100%, 50% 50%);
    }
    .ghost {
        width: 30px;
        height: 30px;
        background: red;
        position: absolute;
        border-radius: 15px 15px 0 0;
    }
    .pellet {
        width: 10px;
        height: 10px;
        background: white;
        position: absolute;
        border-radius: 50%;
    }
    .score {
        color: black;
        font-family: Arial;
        padding: 10px;
        text-align: center;
    }
</style>

<script>
    const gameBoard = document.getElementById('game-board');
    const pacman = document.getElementById('pacman');
    const scoreElement = document.getElementById('score');
    let score = 0;
    let pacmanX = 0;
    let pacmanY = 0;
    
    // Game settings
    const GAME_SIZE = 500;
    const MOVE_STEP = 20;
    
    // Create pellets
    for (let i = 0; i < 15; i++) {
        for (let j = 0; j < 15; j++) {
            const pellet = document.createElement('div');
            pellet.className = 'pellet';
            pellet.style.left = `${i * 30 + 10}px`;
            pellet.style.top = `${j * 30 + 10}px`;
            gameBoard.appendChild(pellet);
        }
    }
    
    // Create ghost
    const ghost = document.createElement('div');
    ghost.className = 'ghost';
    ghost.style.left = '400px';
    ghost.style.top = '400px';
    gameBoard.appendChild(ghost);
    
    // Move pacman
    document.addEventListener('keydown', (e) => {
        switch(e.key) {
            case 'ArrowLeft':
                if (pacmanX > 0) pacmanX -= MOVE_STEP;
                pacman.style.transform = 'rotate(180deg)';
                break;
            case 'ArrowRight':
                if (pacmanX < GAME_SIZE - 30) pacmanX += MOVE_STEP;
                pacman.style.transform = 'rotate(0deg)';
                break;
            case 'ArrowUp':
                if (pacmanY > 0) pacmanY -= MOVE_STEP;
                pacman.style.transform = 'rotate(270deg)';
                break;
            case 'ArrowDown':
                if (pacmanY < GAME_SIZE - 30) pacmanY += MOVE_STEP;
                pacman.style.transform = 'rotate(90deg)';
                break;
        }
        
        pacman.style.left = pacmanX + 'px';
        pacman.style.top = pacmanY + 'px';
        
        // Check pellet collection
        const pellets = document.getElementsByClassName('pellet');
        Array.from(pellets).forEach(pellet => {
            const pelletRect = pellet.getBoundingClientRect();
            const pacmanRect = pacman.getBoundingClientRect();
            
            if (isColliding(pacmanRect, pelletRect)) {
                pellet.remove();
                score += 10;
                scoreElement.textContent = score;
            }
        });
        
        // Check ghost collision
        const ghostRect = ghost.getBoundingClientRect();
        const pacmanRect = pacman.getBoundingClientRect();
        
        if (isColliding(pacmanRect, ghostRect)) {
            alert('Game Over! Your score: ' + score);
            location.reload();
        }
    });
    
    // Move ghost
    setInterval(() => {
        const ghostX = parseInt(ghost.style.left);
        const ghostY = parseInt(ghost.style.top);
        
        // Simple ghost AI
        if (ghostX < pacmanX) ghost.style.left = (ghostX + 10) + 'px';
        if (ghostX > pacmanX) ghost.style.left = (ghostX - 10) + 'px';
        if (ghostY < pacmanY) ghost.style.top = (ghostY + 10) + 'px';
        if (ghostY > pacmanY) ghost.style.top = (ghostY - 10) + 'px';
    }, 200);
    
    function isColliding(rect1, rect2) {
        return !(rect1.right < rect2.left || 
                rect1.left > rect2.right || 
                rect1.bottom < rect2.top || 
                rect1.top > rect2.bottom);
    }
</script>


## Technical Implementation
The game demonstrates basic game development concepts including:
- DOM manipulation
- Event handling
- Collision detection
- Simple AI for ghost movement
- Game state management
