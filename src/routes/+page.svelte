<script lang="ts">
  import Scene from "$lib/components/Scene.svelte";
  import { Canvas } from "@threlte/core";
</script>

<div class="bg">
  <Canvas>
    <Scene />
  </Canvas>
</div>

<style>
  .bg {
    background-color: #12c4c4;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    overflow: hidden;
  }
</style>
