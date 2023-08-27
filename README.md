This is a simple HTML, CSS, and JavaScript code that allows a user to Delete and undo to a webpage.
Hosted Link :

### Step-by-Step Explanation

#### HTML 
The HTML code creates a simple webpage with a text input, a button, and a div to hold the paragraphs.
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div id="controls">
      <button id="deleteButton">Delete</button>
      <button id="undoButton">Undo</button>
    </div>
    <canvas id="whiteboard" width="800" height="600"></canvas>
    <script src="./index.js"></script>
  </body>
</html>
```
#### JavaScript
```JavaScript
const canvas = document.getElementById("whiteboard");
const deleteButton = document.getElementById("deleteButton");
const undoButton = document.getElementById("undoButton");
const ctx = canvas.getContext("2d");
let drawing = false;
let objects = [];
let tempPath = null;

canvas.addEventListener("mousedown", startDrawing);
canvas.addEventListener("mousemove", draw);
canvas.addEventListener("mouseup", stopDrawing);
canvas.addEventListener("mouseout", stopDrawing);

deleteButton.addEventListener("click", clearWhiteboard);
undoButton.addEventListener("click", handleUndo);

function startDrawing(e) {
  drawing = true;
  tempPath = [
    { x: e.clientX - canvas.offsetLeft, y: e.clientY - canvas.offsetTop },
  ];
}

function draw(e) {
  if (!drawing) return;
  ctx.lineWidth = 3;
  ctx.lineCap = "round";
  ctx.strokeStyle = "black";

  tempPath.push({
    x: e.clientX - canvas.offsetLeft,
    y: e.clientY - canvas.offsetTop,
  });

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  redrawCanvas();
}

function stopDrawing() {
  if (!drawing) return;
  drawing = false;
  if (tempPath.length > 1) {
    addObjectToCanvas(tempPath);
  }
  tempPath = null;
}

function clearWhiteboard() {
  objects = [];
  redrawCanvas();
}

function handleUndo() {
  objects.pop(); // Remove the last drawn object
  redrawCanvas();
}

function redrawCanvas() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  for (const obj of objects) {
    ctx.beginPath();
    ctx.moveTo(obj.path[0].x, obj.path[0].y);
    for (const point of obj.path) {
      ctx.lineTo(point.x, point.y);
    }
    ctx.stroke();
  }
}

function addObjectToCanvas(path) {
  objects.push({ path });
  redrawCanvas();
}
```
