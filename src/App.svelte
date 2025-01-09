<script>
  import { onMount } from 'svelte';
  
  let ball;
  const random = (min, max) => Math.random() * (max - min) + min;
  
  // Adjusted starting position for new logo size
  let x = random(50, 400);
  let y = random(50, 500);
  let dx = random(-3, 3);  // Increased speed slightly for larger area
  let dy = random(-3, 3);
  
  if (Math.abs(dx) < 0.8) dx = dx < 0 ? -2 : 2;  // Increased minimum speed
  if (Math.abs(dy) < 0.8) dy = dy < 0 ? -2 : 2;

  // Colors for DVD logo (using hue-rotate values)
  const colors = [0, 60, 120, 180, 240, 300];
  let currentColorIndex = 0;

  function changeColor() {
    currentColorIndex = (currentColorIndex + 1) % colors.length;
    ball.style.filter = `hue-rotate(${colors[currentColorIndex]}deg)`;
  }

  function animate() {
    x += dx;
    y += dy;
    
    // Adjusted collision boundaries for 236x134 logo
    if (x >= (800 - 236) || x <= 0) {
      dx = -dx;
      changeColor();
    }
    if (y >= (730 - 134) || y <= 0) {
      dy = -dy;
      changeColor();
    }
    
    ball.style.transform = `translate(${x}px, ${y}px)`;
    requestAnimationFrame(animate);
  }
  
  onMount(() => {
    ball.style.filter = `hue-rotate(${colors[currentColorIndex]}deg)`;
    animate();
  });
</script>

<main>
  <div class="container">
    <img 
      class="dvd" 
      bind:this={ball}
      src="DVD.png"
      alt="DVD Logo"
    />
  </div>
</main>

<style>
  .container {
    width: 700px;
    height: 700px;
    border: 4px solid #333;  /* Made border thicker */
    position: relative;
    margin: 0px auto;
    background: #000;
  }
  
  .dvd {
    width: 20%;
    height: 134px;
    position: absolute;
    top: 0;
    left: 0;
    filter: hue-rotate(0deg);
  }
</style>
