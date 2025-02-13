Hello World

## Pacman

```js client
<script src='https://code.jquery.com/jquery-1.4.2.min.js'></script>
<script>
    document.addEventListener('DOMContentLoaded', (event) => {
        const grid = document.createElement('div');
        grid.style.display = 'grid';
        grid.style.gridTemplateRows = 'repeat(28, 20px)';
        grid.style.gridTemplateColumns = 'repeat(36, 20px)';
        grid.style.gap = '1px';
        document.body.appendChild(grid);

        for (let i = 0; i < 28 * 36; i++) {
            const cell = document.createElement('div');
            cell.style.width = '20px';
            cell.style.height = '20px';
            cell.style.border = '1px solid #ccc';
            grid.appendChild(cell);
        }

        const yellowBox = document.createElement('div');
        yellowBox.style.width = '20px';
        yellowBox.style.height = '20px';
        yellowBox.style.backgroundColor = 'yellow';
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
```