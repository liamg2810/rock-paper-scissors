<script lang="ts">
  import { T } from "@threlte/core";
  import { createEventDispatcher, onMount } from "svelte";
  import { cubicInOut } from "svelte/easing";
  import { tweened, type Tweened } from "svelte/motion";
  import { Group } from "three";
  import Hand from "./Hand.svelte";
  const dispatch = createEventDispatcher();

  export const ref = new Group();

  const gamePosition = tweened(-50, { duration: 1000, easing: cubicInOut });

  let started = false;

  export async function Start() {
    gamePosition.set(-50, { duration: 0, easing: cubicInOut });
    started = true;
    await gamePosition.set(0);
  }

  const done = async () => {
    await gamePosition.set(-50);
  };

  const shakeCamera = async () => {
    dispatch("shakeCamera");
  };

  onMount(() => {
    Start();
  });
</script>

<T is={ref} position={[0, $gamePosition, 0]} visible={started}>
  <Hand
    position={[-10, 5, 5]}
    rotY={Math.PI * 2.25}
    on:shakeCamera={shakeCamera}
    on:done={done}
    scale={1.5}
  />

  <T.Mesh position={[0, 0, 0]} receiveShadow>
    <T.BoxGeometry args={[100, 0.1, 100]} />
    <T.MeshStandardMaterial color="#444444" />
  </T.Mesh>
</T>
