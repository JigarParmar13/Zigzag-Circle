<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zigzag Circle Movement</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #f0f0f0;
        }
        #container {
            width: 500px;
            height: 500px;
            border: 3px solid black;
            position: relative;
            background-color: white;
        }
        #circle {
            width: 50px;
            height: 50px;
            background-color: red;
            border-radius: 50%;
            position: absolute;
            bottom: 0;
            left: 0;
        }
        .controls {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div id="container">
        <div id="circle"></div>
    </div>
    <div class="controls">
        <label>Time (seconds): <input type="number" id="timeInput" value="5"></label>
        <label>Circle Color: 
            <select id="colorPicker">
                <option value="red">Red</option>
                <option value="blue">Blue</option>
                <option value="green">Green</option>
                <option value="yellow">Yellow</option>
            </select>
        </label>
        <button onclick="startMovement()">Start</button>
        <button onclick="stopMovement()">Stop</button>
        <button onclick="resetMovement()">Reset</button>
        <p>Current Row: <span id="rowCounter">0</span></p>
    </div>

    <script>
        const circle = document.getElementById("circle");
        const container = document.getElementById("container");
        const timeInput = document.getElementById("timeInput");
        const rowCounter = document.getElementById("rowCounter");
        const colorPicker = document.getElementById("colorPicker");

        let interval;
        let row = 0;
        let col = 0;
        let stepSize = 50;
        let speed = 100;
        let isMovingRight = true;
        let isRunning = false;

        function startMovement() {
            if (isRunning) return;
            isRunning = true;
            let totalTime = parseFloat(timeInput.value) * 1000;
            let totalRows = container.clientHeight / stepSize;
            speed = totalTime / (totalRows * (container.clientWidth / stepSize));

            interval = setInterval(moveCircle, speed);
        }

        function stopMovement() {
            clearInterval(interval);
            isRunning = false;
        }

        function resetMovement() {
            stopMovement();
            row = 0;
            col = 0;
            isMovingRight = true;
            updatePosition();
            rowCounter.textContent = "0";
        }

        function moveCircle() {
            let maxCols = container.clientWidth / stepSize;
            let maxRows = container.clientHeight / stepSize;
            
            if (isMovingRight) {
                if (col < maxCols - 1) {
                    col++;
                } else {
                    row++;
                    isMovingRight = false;
                }
            } else {
                if (col > 0) {
                    col--;
                } else {
                    row++;
                    isMovingRight = true;
                }
            }
            if (row >= maxRows) {
                stopMovement();
            } else {
                updatePosition();
                rowCounter.textContent = row;
            }
        }

        function updatePosition() {
            circle.style.left = col * stepSize + "px";
            circle.style.bottom = row * stepSize + "px";
        }

        colorPicker.addEventListener("change", () => {
            circle.style.backgroundColor = colorPicker.value;
        });
    </script>
</body>
</html>
