<script lang="ts">
import { onMount } from "svelte";
let sizeEL;
let canvas;
let size = 10;
let isPressed = false;
let color = "black";
let x;
let y;
let ctx;

onMount(() => {
  ctx = canvas.getContext("2d");
  sizeEL.innerText = size;
});

function onCanvasMouseDown(e) {
  isPressed = true;

  x = e.offsetX;
  y = e.offsetY;
}

function onCanvasMouseUp(e) {
  isPressed = false;

  x = undefined;
  y = undefined;
}

function onCanvasMouseMove(e) {
  if (isPressed) {
    const x2 = e.offsetX;
    const y2 = e.offsetY;

    drawCircle(x2, y2);
    drawLine(x, y, x2, y2);

    x = x2;
    y = y2;
  }
}

function drawCircle(x, y) {
  ctx.beginPath();
  ctx.arc(x, y, size, 0, Math.PI * 2);
  ctx.fillStyle = color;
  ctx.fill();
}

function drawLine(x1, y1, x2, y2) {
  ctx.beginPath();
  ctx.moveTo(x1, y1);
  ctx.lineTo(x2, y2);
  ctx.strokeStyle = color;
  ctx.lineWidth = size * 2;
  ctx.stroke();
}

function updateSizeOnScreen() {
  sizeEL.innerText = size;
}

function onClickIncrease() {
  size += 5;

  if (size > 50) {
    size = 50;
  }

  updateSizeOnScreen();
}

function onClickDecrease() {
  size -= 5;

  if (size < 5) {
    size = 5;
  }

  updateSizeOnScreen();
}

function onClickClear() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
}
</script>

<main>
  <canvas
    bind:this={canvas}
    on:mousedown={onCanvasMouseDown}
    on:mouseup={onCanvasMouseUp}
    on:mousemove={onCanvasMouseMove}
    width="800"
    height="700" />
  <div class="toolbox">
    <button on:click={onClickDecrease}>-</button>
    <span bind:this={sizeEL} />
    <button on:click={onClickIncrease}>+</button>
    <input
      type="color"
      bind:value={color}
      on:change={(e) => (color = e.target.value)} />
    <button id="clear" on:click={onClickClear}>Clear</button>
  </div>
</main>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

main {
  background-color: white;
  font-family: "Roboto", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
}

canvas {
  border: 2px solid steelblue;
}

.toolbox {
  background-color: steelblue;
  border: 1px solid slateblue;
  display: flex;
  width: 804px;
  padding: 1rem;
}

.toolbox > * {
  background-color: #fff;
  border: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  height: 50px;
  width: 50px;
  margin: 0.25rem;
  padding: 0.25rem;
  cursor: pointer;
}

.toolbox > *:last-child {
  margin-left: auto;
}
</style>
