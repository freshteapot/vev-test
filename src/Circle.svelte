<script>
  import { createEventDispatcher } from "svelte";
  export let side;
  export let coords;
  const dispatch = createEventDispatcher();
  const dragging = {
    x: 0,
    y: 0,
    dx: 0,
    dy: 0
  };

  function handleDragStart(event) {
    dragging.x = event.clientX;
    dragging.y = event.clientY;
  }
  function handleDragOver(event) {
    event.preventDefault();
  }

  function handleDrag(event) {
    event.preventDefault();
    dragging.dx = event.clientX - dragging.x;
    dragging.dy = event.clientY - dragging.y;
    dragging.x = event.clientX;
    dragging.y = event.clientY;

    dispatch("onmove", {
      x: coords.x + dragging.dx,
      y: coords.y + dragging.dy,
      side: side
    });
  }

  function handleChangeX(event) {
    let x = Math.round(event.target.value);
    dispatch("onmove", {
      x: x,
      y: coords.y,
      side: side
    });
  }

  function handleChangeY(event) {
    let y = Math.round(event.target.value);
    dispatch("onmove", {
      x: coords.x,
      y: y,
      side: side
    });
  }
</script>

<style>
  .box {
    z-index: 1;
    border-radius: 50%;
    --width: 150px;
    --height: 150px;
    position: absolute;
    width: var(--width);
    height: var(--height);
    background-color: #ff705a;
    cursor: move;
    text-align: center;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    top: 0px;
    left: 0px;
  }

  span {
    color: #001733;
    display: block;
    margin: 8px;
  }
  input {
    background: 0 0;
    width: 60px;
    border: none;
    border-bottom: 1px solid #001733;
    font: 400 9px system-ui;
    text-align: center;
  }
</style>

<div class="box"
  draggable="true"
  on:dragstart={handleDragStart}
  on:drag={handleDrag}
  on:dragover={handleDragOver}
	style="transform:translate({coords.x}px,{coords.y}px);"
>
  <span>X<input type="number" value={coords.x} on:change={handleChangeX}></span>
  <span>Y<input type="number" value={coords.y} on:change={handleChangeY}></span>
</div>
