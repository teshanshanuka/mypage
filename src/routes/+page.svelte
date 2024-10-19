<script lang="ts">
  import { onMount } from 'svelte';
  import { tweened, type Tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';

  const rand = (min: number, max: number) => Math.floor(Math.random() * (max - min)) + min;
  const randChoice = (arr: any[]) => arr[Math.floor(Math.random() * arr.length)];
  const trimToRange = (val: number, min: number, max: number) => Math.max(Math.min(val, max), min);

  let mycanvas: HTMLDivElement;
  let width: number, height: number, stepX: number, stepY: number;
  let margin = 10;
  let freq = 5;
  let step = 10;

  let blobX = tweened(undefined, {
    duration: 400,
    easing: cubicOut,
  }) as unknown as Tweened<number>;
  let blobY = tweened(null, {
    duration: 400,
    easing: cubicOut,
  }) as unknown as Tweened<number>;

  onMount(() => {
    ({ width, height } = mycanvas.getBoundingClientRect());
    width -= 2 * margin;
    height -= 2 * margin;
    blobX.set(rand(margin, margin + width));
    blobY.set(rand(margin, margin + height));
  });

  let clear: number;
  const updateAnim = () => {
    clearInterval(clear);
    const stepT = (1 / freq) * 1000;

    clear = setInterval(() => {
      stepX = randChoice([-step, 0, step]);
      stepY = randChoice([-step, 0, step]);

      blobX.set(trimToRange($blobX + stepX, margin, width - margin), { duration: stepT });
      blobY.set(trimToRange($blobY + stepY, margin, height - margin), { duration: stepT });
    }, stepT);
  }
  updateAnim();
</script>

<title>Teshan's stuff</title>

<body>
  <div class="welcome"><span class="blink">\></span>Hola!</div>
  <div class="container">
    <div class="mycanvas" bind:this={mycanvas}>
      <div class="blob" style="left: {$blobX}px; top: {$blobY}px;"></div>
    </div>

    <div class="range-container">
      <input type="range" min="0.1" max="50" step="0.1" bind:value={freq} on:input={updateAnim} class="range-slider" />
      <span class="range-text">{freq}Hz</span>
      <input type="range" min="1" max="100" step="1" bind:value={step} on:input={updateAnim} class="range-slider" />
      <span class="range-text">step: {step}</span>
    </div>
  </div>
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
    0% {
      opacity: 1;
    }
    50% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }

  .container {
    width: 70%;
    height: 60%;
    position: absolute;
    left: 15%;
    top: 20%;
  }
  .mycanvas:hover {
    box-shadow: 0 0 40px 0 rgba(0, 0, 0, 0.5);
    transform: scale(1.01);
  }
  .mycanvas {
    width: 100%;
    height: 90%;
    position: absolute;
    border-radius: 20px;
    background-color: black;
    box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
    top: 0;
    transition: all 0.15s ease-in-out;
  }

  .range-container {
    display: flex;
    align-items: center; /* Align the slider and text vertically */
    position: absolute;
    bottom: 0;
  }

  .range-slider {
    margin-right: 10px; /* Add some space between the slider and the text */
  }
  .range-text {
    font-size: 1.5em;
    margin-right: 20px;
  }

  .blob {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: white;
    position: absolute;
  }

  body {
    margin: 0;
    padding: 0;
    border-radius: 5px;
    background: linear-gradient(135deg, #6a11cb, #c608df); /* Blue/Purple gradient */
    font-family: monospace;
  }
</style>
