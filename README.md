<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>The Impossible Lightbulb | @coding.stella</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --depth: 5vmin;
  --on: 0;
  --size: 25vmin;
  --bg: hsl(calc(200 - (var(--on) * 160)), calc((20 + (var(--on) * 50)) * 1%), calc((20 + (var(--on) * 60)) * 1%));
  --cord: hsl(0, 0%, calc((60 - (var(--on) * 50)) * 1%));
  --stroke: hsl(0, 0%, calc((60 - (var(--on) * 50)) * 1%));
  --shine: hsla(0, 0%, 100%, calc(0.75 - (var(--on) * 0.5)));
  --cap: hsl(0, 0%, calc((40 + (var(--on) * 30)) * 1%));
  --filament: hsl(45, calc(var(--on) * 80%), calc((25 + (var(--on) * 75)) * 1%));
}
body {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg);
  overflow: hidden;
}
.toggle {
  position: relative;
  width: var(--size);
  height: var(--size);
}
.toggle input {
  position: absolute;
  opacity: 0;
}
svg {
  width: 100%;
  height: auto;
}
.bulb__filament {
  stroke: var(--filament);
  stroke-width: 5;
}
.bulb__bulb {
  stroke: var(--stroke);
  fill: hsla(calc(180 - (95 * var(--on))), 80%, 80%, calc(0.1 + (0.4 * var(--on))));
  stroke-width: 5;
}
.bulb__cap {
  fill: var(--cap);
}
.bulb__shine {
  stroke: var(--shine);
}
.toggle-scene__cord {
  stroke: var(--cord);
  stroke-width: 6;
}
.toggle-scene__hit-spot {
  cursor: grab;
}
</style>
</head>
<body>

<div class="toggle">
  <input type="checkbox" id="light-mode" />
  <svg class="toggle-scene" viewBox="0 0 200 400">
    <line x1="100" y1="0" x2="100" y2="200" class="toggle-scene__cord" />
    <circle cx="100" cy="200" r="20" class="toggle-scene__hit-spot" fill="transparent" />
    <g class="bulb" transform="translate(100,250)">
      <circle r="40" class="bulb__bulb" fill="rgba(255, 255, 200, 0.2)" stroke="black"/>
      <line y1="-20" y2="20" class="bulb__filament" />
      <circle r="10" cy="-50" class="bulb__cap" />
      <line x1="-20" y1="-60" x2="20" y2="-60" class="bulb__shine" stroke-width="3" />
    </g>
  </svg>
</div>

<script src="https://unpkg.com/gsap@3/dist/gsap.min.js"></script>
<script src="https://unpkg.com/gsap@3/dist/Draggable.min.js"></script>
<script>
const input = document.querySelector("#light-mode");
const hitSpot = document.querySelector(".toggle-scene__hit-spot");
const bulb = document.querySelector(".bulb");
const root = document.documentElement;
let isOn = false;

function toggleLight() {
  isOn = !isOn;
  input.checked = isOn;
  root.style.setProperty('--on', isOn ? 1 : 0);
}

Draggable.create(hitSpot, {
  type: "y",
  bounds: { minY: -50, maxY: 50 },
  onDragEnd: function() {
    if (Math.abs(this.y) > 30) {
      toggleLight();
      gsap.to(this.target, { y: 0, duration: 0.5, ease: "elastic.out(1, 0.3)" });
    } else {
      gsap.to(this.target, { y: 0, duration: 0.3 });
    }
  }
});
</script>
</body>
</html>
