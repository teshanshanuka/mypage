<script lang="ts">
  import { onMount } from 'svelte';
  import { tweened, type Tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';

  const rand = (min: number, max: number) => Math.floor(Math.random() * (max - min)) + min;
  const randChoice = (arr: any[]) => arr[Math.floor(Math.random() * arr.length)];
  const trimToRange = (val: number, min: number, max: number) => Math.max(Math.min(val, max), min);

  let svg: SVGSVGElement;
  let width: number, height: number, prevX: number, prevY: number;
  let blobR = 8;
  let freq = 5;
  let step = 10;
  let polylnPts = '';
  let paused = false;

  let blobX = tweened(undefined, {
    duration: 400,
    easing: cubicOut,
  }) as unknown as Tweened<number>;
  let blobY = tweened(undefined, {
    duration: 400,
    easing: cubicOut,
  }) as unknown as Tweened<number>;

  onMount(() => {
    blobX.set(rand(blobR, width - 2 * blobR));
    blobY.set(rand(blobR, height - 2 * blobR));

    prevX = $blobX;
    prevY = $blobY;
    polylnPts = `${$blobX},${$blobY}`;
  });

  let clear: number;
  const updateAnim = () => {
    clearInterval(clear);
    const stepT = (1 / freq) * 1000;

    clear = setInterval(() => {
      if (!$blobX || !$blobY) return;

      const stepX = randChoice([-step, 0, step]);
      const stepY = randChoice([-step, 0, step]);

      blobX.set(trimToRange($blobX + stepX, blobR, width - 2 * blobR), { duration: stepT });
      blobY.set(trimToRange($blobY + stepY, blobR, height - 2 * blobR), { duration: stepT });

      prevX = $blobX;
      prevY = $blobY;
      polylnPts += `, ${$blobX} ${$blobY}`;
    }, stepT);
  };
  updateAnim();

  $: paused ? clearInterval(clear) : updateAnim();
</script>

<title>Teshan's stuff</title>

<main class="main">
  <div class="welcome"><span class="blink">\></span>Hola!</div>
  <div class="container">
    <div class="mycanvas" bind:clientWidth={width} bind:clientHeight={height}>
      <div
        class="blob"
        style="left: {$blobX - blobR}px; top: {$blobY - blobR}px; width: {blobR * 2}px; height: {blobR * 2}px"
      ></div>
      <svg bind:this={svg} viewBox="0 0 {width} {height}" {width} {height}>
        <polyline points={polylnPts} style="stroke:white;stroke-width:2;stroke-opacity:0.6" />
        <line x1={prevX} y1={prevY} x2={$blobX} y2={$blobY} style="stroke:white;stroke-width:2" />
      </svg>
    </div>

    <div class="range-container">
      <input type="range" min="0.1" max="50" step="0.1" bind:value={freq} on:input={updateAnim} class="range-slider" />
      <span class="range-text">{freq}Hz</span>

      <input type="range" min="1" max="100" step="1" bind:value={step} on:input={updateAnim} class="range-slider" />
      <span class="range-text">step: {step}</span>

      <button on:click={() => (paused = !paused)}>{paused ? 'Resume' : 'Pause'}</button>
      <button on:click={() => (polylnPts = `${$blobX},${$blobY}`)} style="align-self: flex-end;">Clear path</button>
    </div>
  </div>
</main>

<style>
  .welcome {
    text-align: left;
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

  button {
    background-color: rgba(219, 219, 219, 0.24);
    border-radius: 8px;
    border-width: 0;
    color: #1b1b1b;
    cursor: pointer;
    display: inline-block;
    font-size: 14px;
    margin: 0;
    padding: 10px 12px;
    text-align: center;
    vertical-align: baseline;
    margin: 0 10px 0 0;
  }

  .blob {
    border-radius: 50%;
    background-color: white;
    position: absolute;
    animation: pulse 0.3s infinite; /* Add animation */
  }

  @keyframes pulse {
    0% {
      box-shadow:
        0 0 5px white,
        0 0 10px white,
        0 0 15px white;
      transform: scale(1);
    }
    50% {
      box-shadow:
        0 0 2px white,
        0 0 5px white,
        0 0 10px white;
      transform: scale(1.1);
    }
    100% {
      box-shadow:
        0 0 5px white,
        0 0 10px white,
        0 0 15px white;
      transform: scale(1);
    }
  }

  .main {
    margin: 0;
    padding: 0;
    border-radius: 5px;
    background: linear-gradient(135deg, #6a11cb, #c608df); /* Blue/Purple gradient */
    font-family: monospace;
    height: 100vh;
    overflow: hidden;
    font-family: monospace;
  }
</style>
