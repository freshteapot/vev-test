<script>
	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();

	export let coords;

	function handleChange(event) {
	  let distance = Math.round(event.target.value);
	  dispatch("onmove", {
	    distance: distance,
	    side: "line"
	  });
	}
</script>
<style>
	div {
	  background: #001733;
	  height: 2px;
	  -webkit-transform-origin: 0 0;
	  transform-origin: 0 0;
	  transform: none;
	  position: absolute;
	  width: 500px;
	}
	input {
	  position: absolute;
	  z-index: 100;
	  left: 50%;
	  background: #001733;
	  margin-top: -15px;
	  margin-left: -30px;
	  color: #fff;
	  padding: 10px;
	  border-radius: 10px;
	  width: 60px;
	  -webkit-writing-mode: horizontal-tb !important;
	  box-sizing: inherit;
	  -webkit-rtl-ordering: logical;
	  cursor: text;
	  text-align: center;
	  font: 400 11px system-ui;
	}
</style>

<div id="iamtheline" style="transform:{coords.transformRotate};left:{coords.x}px;top:{coords.y}px;width:{coords.distance}px;">
	<input type="number" value={Math.round(coords.distance)} on:change={handleChange}>>
</div>
