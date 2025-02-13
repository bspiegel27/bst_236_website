Hello World

## Pacman
```html
<canvas id="gameCanvas" width="500" height="500" style="border:1px solid #000000;"></canvas>

<script>
    var canvas = document.getElementById("gameCanvas");
    var ctx = canvas.getContext("2d");
    var box = {
        x: 50,
        y: 50,
        size: 20,
        color: "black"
    };

    document.addEventListener("keydown", moveBox);

    function moveBox(e) {
        switch(e.key) {
            case "ArrowUp":
                box.y -= 5;
                break;
            case "ArrowDown":
                box.y += 5;
                break;
            case "ArrowLeft":
                box.x -= 5;
                break;
            case "ArrowRight":
                box.x += 5;
                break;
        }
        drawBox();
    }

    function drawBox() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = box.color;
        ctx.fillRect(box.x, box.y, box.size, box.size);
    }

    drawBox();
</script>
```