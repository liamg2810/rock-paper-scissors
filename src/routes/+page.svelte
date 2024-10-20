<script lang="ts">
  import Scene from "$lib/components/Scene.svelte";
  import { Canvas } from "@threlte/core";
</script>

<div class="bg">
  <div class="lives">
    <span>❤️</span><span> : 3</span>
  </div>
  <Canvas>
    <Scene />
  </Canvas>
</div>

<style>
  .bg {
    background-color: #111111;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    overflow: hidden;
  }

  .lives {
    pointer-events: none;
    -webkit-user-select: none;
    user-select: none;
    position: absolute;
    top: 10px;
    left: 10px;
    color: white;
    font-size: 4rem;
  }
</style>
