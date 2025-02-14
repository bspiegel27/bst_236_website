# Building a Simple Pacman Game with JavaScript

*Posted on January 15, 2024*

Recently, I completed a fun project implementing the classic Pacman game using vanilla JavaScript. This project helped me understand core concepts of game development including collision detection, character movement, and simple AI behavior.

The game features a basic implementation where Pacman can move around collecting pellets while being chased by a ghost. The ghost uses a simple AI algorithm to follow Pacman's position. One of the interesting challenges was implementing the collision detection system between Pacman, the pellets, and the ghost.

Here's how I approached the collision detection:
```javascript
function isColliding(rect1, rect2) {
    return !(rect1.right < rect2.left || 
            rect1.left > rect2.right || 
            rect1.bottom < rect2.top || 
            rect1.top > rect2.bottom);
}
```

This function checks if two rectangles (representing game elements) overlap. It's a simple but effective way to detect when Pacman collects a pellet or gets caught by the ghost.

[Try the game yourself](/course_content/pacman.md)

*Tags: #javascript #gamedev #webdev*

