<script>
  import Spirograph from "./Spirograph.svelte";

  let width;
  let height;
</script>

<svelte:window bind:innerWidth={width} bind:innerHeight={height} />

<Spirograph {width} {height} />
