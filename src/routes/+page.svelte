<script lang="ts">
  import { onMount } from 'svelte';

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;

  onMount(() => {
    ctx = canvas.getContext('2d') as CanvasRenderingContext2D;

    // Set the canvas size to match the CSS-rendered size
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;

    // Generate random position for the circle
    const randomX = Math.floor(Math.random() * (canvas.width - 20)) + 10; // Ensure it stays within bounds
    const randomY = Math.floor(Math.random() * (canvas.height - 20)) + 10;

    // Draw the white circle
    function drawCircle() {
      ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear previous drawing
      ctx.beginPath();
      ctx.arc(randomX, randomY, 10, 0, Math.PI * 2, false); // Circle with radius 10
      ctx.fillStyle = 'white';
      ctx.fill();
      ctx.closePath();
    }

    drawCircle();
    console.log(randomX, randomY);
  });
</script>

<title>Teshan's stuff</title>

<body>
  <div class="welcome"><span class="blink">\></span>Hola!</div>
  <canvas class="window" bind:this={canvas}> </canvas>
</body>

<style>
  .welcome {
    text-align: left;
    font-family: monospace;
    font-size: 2.5em;
  }

  .blink {
    animation: blink-animation 1s steps(1, end) infinite;
  }

  @keyframes blink-animation {
    0% { opacity: 1; }
    50% { opacity: 0; }
    100% { opacity: 1; }
  }

  .window {
    width: 70%;
    height: 60%;
    background-color: black;
    position: absolute;
    left: 15%;
    top: 20%;
    border-radius: 20px;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
    transition: all 0.15s ease-in-out;
  }
  .window:hover {
    box-shadow: 0 0 40px 0 rgba(0, 0, 0, 0.5);
    transform: scale(1.01);
  }

  body {
    margin: 0;
    padding: 0;
    border-radius: 5px;
    background: linear-gradient(135deg, #6a11cb, #c608df); /* Blue/Purple gradient */
  }
</style>
