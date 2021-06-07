<script lang="ts">
import { onMount } from "svelte";
let sizeEL: HTMLSpanElement;
let canvas: HTMLCanvasElement;
let size = 10;
let isPressed = false;
let color = "black";
let x: number;
let y: number;
let ctx: CanvasRenderingContext2D;
let hue = 0;
let isRainbowClicked = false;

onMount(() => {
  ctx = canvas.getContext("2d");
  sizeEL.innerText = size.toString();
});

function onCanvasMouseDown(e: MouseEvent) {
  isPressed = true;

  x = e.offsetX;
  y = e.offsetY;
}

function onCanvasMouseUp() {
  isPressed = false;

  x = undefined;
  y = undefined;
}

function onCanvasMouseMove(e: MouseEvent) {
  if (isPressed) {
    const x2 = e.offsetX;
    const y2 = e.offsetY;

    drawCircle(x2, y2);
    drawLine(x, y, x2, y2);

    x = x2;
    y = y2;
  }
}

function drawCircle(x: number, y: number) {
  ctx.beginPath();
  ctx.arc(x, y, size, 0, Math.PI * 2);
  ctx.fillStyle = color;
  ctx.fill();
}

function drawLine(x1: number, y1: number, x2: number, y2: number) {
  ctx.beginPath();
  ctx.moveTo(x1, y1);
  ctx.lineTo(x2, y2);
  ctx.strokeStyle = color;
  ctx.lineWidth = size * 2;
  ctx.stroke();

  if (isRainbowClicked) {
    hue++;
    if (hue >= 360) {
      hue = 0;
    }
    color = `hsl(${hue}, 100%, 50%)`;
  }
}

function updateSizeOnScreen() {
  sizeEL.innerText = size.toString();
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

function onChangeColor(e: any) {
  const target = e.target as HTMLInputElement;
  color = target.value;
}

function onClickColor() {
  isRainbowClicked = false;
}

function onClickRainbowColor() {
  isRainbowClicked = true;
  color = `hsl(0, 100%, 50%)`;
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
      on:change={onChangeColor}
      on:click={onClickColor} />
    <button on:click={onClickRainbowColor}>🌈</button>
    <button on:click={onClickClear} class="clear">Clear</button>
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
  height: 90vh;
  margin: 0;
}

canvas {
  border: 2px solid steelblue;
}

.toolbox {
  background-color: steelblue;
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

.toolbox > .clear {
  padding: 0 3rem;
}

.toolbox > *:last-child {
  margin-left: auto;
}
</style>
