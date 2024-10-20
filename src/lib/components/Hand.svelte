<svelte:options accessors />

<script>
  import { T } from "@threlte/core";
  import { useGltf } from "@threlte/extras";
  import { createEventDispatcher, onMount } from "svelte";
  import { cubicInOut } from "svelte/easing";
  import { tweened } from "svelte/motion";
  import { Group } from "three";
  const dispatch = createEventDispatcher();

  const handGltf = useGltf("assets/rock.gltf");
  export const ref = new Group();

  let yPosition = tweened(0, { duration: 1000, easing: cubicInOut });

  export let rotY = 0;
  export let position = [0, 0, 0];

  onMount(async () => {
    const animate = async () => {
      for (let i = 0; i < 3; i++) {
        await yPosition.set(5);
        await yPosition.set(0, { duration: 100, easing: cubicInOut });
        dispatch("shakeCamera");
      }
    };
    await animate();

    dispatch("done");
  });
</script>

{#if $handGltf}
  <T
    is={ref}
    {...$$restProps}
    {position}
    rotation={[0, rotY, -Math.PI / 2 + $yPosition / 10]}
  >
    <T.Mesh
      geometry={$handGltf.nodes.Cube.geometry}
      material={$handGltf.materials.Skin}
    />
    <T.Mesh
      geometry={$handGltf.nodes.Sleeve.geometry}
      material={$handGltf.materials.Sleeve}
    />
    <slot {ref} />
  </T>
{/if}
