<script>
  import { onMount } from 'svelte';

  let ball;
  const random = (min, max) => Math.random() * (max - min) + min;

  // Initial position and speed of the ball
  let x = random(50, 400);
  let y = random(50, 500);
  let dx = random(-3, 3);
  let dy = random(-3, 3);

  // Ensure minimum speed
  if (Math.abs(dx) < 0.8) dx = dx < 0 ? -2 : 2;
  if (Math.abs(dy) < 0.8) dy = dy < 0 ? -2 : 2;

  // Colors for DVD logo (using hue-rotate values)
  const colors = [0, 60, 120, 180, 240, 300];
  let currentColorIndex = 0;

  // Change the color of the ball
  function changeColor() {
    currentColorIndex = (currentColorIndex + 1) % colors.length;
    ball.style.filter = `hue-rotate(${colors[currentColorIndex]}deg)`;
  }

  // Animate the ball
  function animate() {
    x += dx;
    y += dy;

    // Check for collision with container boundaries
    if (x >= (850 - 256) || x <= 0) {
      dx = -dx;
      changeColor();
    }
    if (y >= (730 - 113) || y <= 0) {
      dy = -dy;
      changeColor();
    }

    ball.style.transform = `translate(${x}px, ${y}px)`;
    requestAnimationFrame(animate);
  }

  // Initialize the animation on component mount
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
    width: 800px;
    height: 730px;
    border: 4px solid #333;  /* Thicker border */
    position: relative;
    margin: 0 auto;
    background: #000;
  }

  .dvd {
    width: 200px;  /* Adjusted width */
    height: 113px;  /* Adjusted height */
    position: absolute;
    top: 0;
    left: 0;
    filter: hue-rotate(0deg);
  }
</style>