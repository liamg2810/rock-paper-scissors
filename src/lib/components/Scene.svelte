<script lang="ts">
  import { T, useFrame } from "@threlte/core";
  import { interactivity, Text } from "@threlte/extras";
  import { onMount } from "svelte";
  import { cubicInOut } from "svelte/easing";
  import { tweened } from "svelte/motion";
  import { writable, type Writable } from "svelte/store";
  import { randInt } from "three/src/math/MathUtils.js";
  import Game from "./Game.svelte";
  interactivity();

  const numSquares = 20;
  const squares: Writable<{ position: number[]; speed: number }[]> = writable(
    []
  );

  let playing = false;

  export let cameraShake = tweened(0, { duration: 100, easing: cubicInOut });

  onMount(() => {
    const initialSquares = Array.from({ length: numSquares }, () => ({
      position: [
        (Math.random() - 0.5) * 200,
        Math.random() * -100 + 50,
        (Math.random() - 0.5) * 100 + 100,
      ],
      speed: Math.random() * 0.1 + 0.01,
    }));
    squares.set(initialSquares);
  });

  useFrame((_, delta) => {
    squares.update((squares) => {
      return squares.map((square) => {
        square.position[1] += square.speed * delta * 100;
        if (square.position[1] > 100) {
          square.position[1] = -100;
        }
        return square;
      });
    });
  });
  const playGame = () => {
    playing = true;
    document.body.style.cursor = "auto";
  };
  const shakeCamera = async () => {
    await cameraShake.set(0.3);
    await cameraShake.set(0);
  };
</script>

<T.PerspectiveCamera
  makeDefault
  position={[
    $cameraShake * randInt(-3, -3),
    15 + $cameraShake * randInt(-3, -3),
    -50,
  ]}
  on:create={({ ref }) => {
    ref.lookAt(0, 10, 0);
  }}
></T.PerspectiveCamera>

<T.AmbientLight intensity={0.3} />

<T.DirectionalLight position={[10, 50, 0]} castShadow />

{#each $squares as square}
  <T.Mesh position={square.position} rotation={[0, Math.PI, 0]}>
    <T.PlaneGeometry args={[5, 5]} />
    <T.MeshBasicMaterial color="#009999" />
  </T.Mesh>
{/each}

{#if playing}
  <Game on:shakeCamera={shakeCamera}></Game>
{:else}
  <T.Mesh
    position={[0, 10, -20]}
    on:click={playGame}
    on:pointerenter={(e) => {
      document.body.style.cursor = "pointer";
    }}
    on:pointerleave={(e) => {
      document.body.style.cursor = "auto";
    }}
    rotation={[0, Math.PI, 0]}
  >
    <T.BoxGeometry args={[10, 5, 1]} />
    <T.MeshStandardMaterial color="blue" />
    <Text
      position={[-4, 1, 0.6]}
      fontSize={1.6}
      color="white"
      text={"Play Game"}
    ></Text>
  </T.Mesh>
{/if}
