<script>
  import { writable } from "svelte/store";
  import Circle from "./Circle.svelte";
  import Line from "./Line.svelte";

  let lineOffsetToCircle = 75;
  let coordsLine = writable({
    x: 0 + lineOffsetToCircle,
    y: 0 + lineOffsetToCircle,
    distance: "0",
    transformRotate: "rotate(0deg)"
  });
  let coordsLeft = writable({ x: 0, y: 0 });
  let coordsRight = writable({ x: 0, y: 0 });

  function handleOnMove(event) {
    if (event.detail.side === "left") {
      // Update the left circle and
      // the line, as the line is anchored
      // to the left circle.
      coordsLeft.update($coordsLeft => ({
        x: event.detail.x,
        y: event.detail.y
      }));

      coordsLine.update($coordsLine => ({
        x: event.detail.x + lineOffsetToCircle,
        y: event.detail.y + lineOffsetToCircle
      }));
    }

    if (event.detail.side === "right") {
      coordsRight.update($coordsRight => ({
        x: event.detail.x,
        y: event.detail.y
      }));
    }

    if (event.detail.side === "line") {
      let point = getPointsFromDistanceAndAngle(event.detail.distance);
      coordsRight.update($coordsRight => ({
        x: point.x,
        y: point.y
      }));
    }
    // Update line with the new distance and roatation.
    coordsLine.update($coordsLine => ({
      x: $coordsLine.x,
      y: $coordsLine.y,
      distance: getLineDistance(),
      transformRotate: getLineRotation()
    }));
  }

  function getPointsFromDistanceAndAngle(distance) {
    var angleDeg =
      (Math.atan2(
        $coordsRight.y - $coordsLeft.y,
        $coordsRight.x - $coordsLeft.x
      ) *
        180) /
      Math.PI;

    let y1 = distance * Math.sin((angleDeg * Math.PI) / 180);
    let x1 = distance * Math.cos((angleDeg * Math.PI) / 180);
    return {
      x: Math.round($coordsLeft.x + x1),
      y: Math.round($coordsLeft.y + y1)
    };
  }

  function getLineDistance() {
    var x1, x2, y1, y2;
    x1 = $coordsLeft.x;
    x2 = $coordsRight.x;
    y1 = $coordsLeft.y;
    y2 = $coordsRight.y;

    var distance = Math.sqrt(Math.pow(x1 - x2, 2) + Math.pow(y1 - y2, 2));
    return distance.toString();
  }

  function getLineRotation() {
    var angleDeg =
      (Math.atan2(
        $coordsRight.y - $coordsLeft.y,
        $coordsRight.x - $coordsLeft.x
      ) *
        180) /
      Math.PI;

    return "rotate(" + angleDeg + "deg)";
  }
</script>

<style>
	div {
	  position: relative;

	  height: 100%;
	  width: 1024px;
	  margin: 0 auto;
	  box-sizing: inherit;
	  -webkit-print-color-adjust: exact;
	  display: block;
	}
</style>

<div>
	<Circle bind:coords={$coordsLeft} side="left" on:onmove={handleOnMove}/>
	<Circle bind:coords={$coordsRight} side="right" on:onmove={handleOnMove}/>
	<Line bind:coords={$coordsLine} on:onmove={handleOnMove}/>
</div>
