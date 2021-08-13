<script>
  // @ts-check

  import { afterUpdate, onMount } from "svelte";

  export let width = 500;
  export let height = width;
  /** @type {[number, number]} */
  $: center = [width * 0.5, height * 0.5];

  /**
   * @param {number} n
   */
  function col(n) {
    return Math.round((Math.sin(n) * 0.5 + 0.5) * 255);
  }

  function line(x1, y1, x2, y2) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.stroke();
  }

  function draw() {
    let a = center;

    for (let i = 0; i < количество_сегментов; i++) {
      a = [
        a[0] + Math.sin(углы[i]) * длины_сегментов[i],
        a[1] + Math.cos(углы[i]) * длины_сегментов[i],
      ];
      углы[i] += сдвиг_углов[i];
    }

    let old_a = [
      a[0] + Math.sin(углы[углы.length - 1]) * ширина_ленточки,
      a[1] + Math.cos(углы[углы.length - 1]) * ширина_ленточки,
    ];

    let f = frameCount * влияние_времени;
    let r = col(углы[углы.length - 1] + f);
    let g = col(углы[углы.length - 2] + f);
    let b = col(углы[углы.length - 3] + f);

    ctx.strokeStyle = `rgb(${r}, ${g}, ${b})`;

    line(a[0], a[1], old_a[0], old_a[1]);

    frameCount++;
  }

  /** @type {HTMLCanvasElement} */
  let canvas;
  /** @type {CanvasRenderingContext2D} */
  let ctx;

  let углы = [];
  let сдвиг_углов = [];
  let длины_сегментов = [];

  let количество_сегментов = 12;
  let диапазон_сдвига_углов = 0.01;
  let диапазон_длины_сегментов = 100;
  let ширина_ленточки = 100;
  let влияние_времени = 0.01;

  for (let i = 0; i < количество_сегментов; i++) {
    углы.push(Math.random() * Math.PI * 2);
    длины_сегментов.push(Math.random() * диапазон_длины_сегментов);
    сдвиг_углов.push((Math.random() - 0.5) * диапазон_сдвига_углов);
  }

  let frameCount = 0;

  onMount(() => {
    function interval() {
      for (let i = 0; i < 1000; i++) {
        draw();
      }
    }
    ctx = canvas.getContext("2d");

    const intervalId = setInterval(interval);
    return () => clearInterval(intervalId);
  });
</script>

<canvas bind:this={canvas} {width} {height} />

<style>
  canvas {
    display: block;
  }
</style>
