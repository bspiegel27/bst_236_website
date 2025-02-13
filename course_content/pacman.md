Hello World

## Pacman

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pacman</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .grid {
            display: grid;
            grid-template-rows: repeat(28, 20px);
            grid-template-columns: repeat(36, 20px);
            gap: 1px;
        }
        .cell {
            width: 20px;
            height: 20px;
            border: 1px solid #ccc;
        }
        .yellow-box {
            width: 20px;
            height: 20px;
            background-color: yellow;
        }
    </style>
</head>
<body>
    <div class="grid"></div>
    <script src='https://code.jquery.com/jquery-1.4.2.min.js'></script>
    <script>
        document.addEventListener('DOMContentLoaded', (event) => {
            const grid = document.querySelector('.grid');

            for (let i = 0; i < 28 * 36; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                grid.appendChild(cell);
            }

            const yellowBox = document.createElement('div');
            yellowBox.classList.add('yellow-box');
            yellowBox.style.gridRowStart = 1;
            yellowBox.style.gridColumnStart = 1;
            grid.appendChild(yellowBox);

            let position = { row: 1, col: 1 };

            document.addEventListener('keydown', (e) => {
                switch (e.key) {
                    case 'ArrowUp':
                        if (position.row > 1) position.row--;
                        break;
                    case 'ArrowDown':
                        if (position.row < 28) position.row++;
                        break;
                    case 'ArrowLeft':
                        if (position.col > 1) position.col--;
                        break;
                    case 'ArrowRight':
                        if (position.col < 36) position.col++;
                        break;
                }
                yellowBox.style.gridRowStart = position.row;
                yellowBox.style.gridColumnStart = position.col;
            });
        });
    </script>
</body>
</html>
```