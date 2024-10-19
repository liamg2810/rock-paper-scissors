<script lang="ts">
  import { T } from "@threlte/core";
  import { createEventDispatcher } from "svelte";
  import Hand from "./Hand.svelte";
  const dispatch = createEventDispatcher();

  const shakeCamera = async () => {
    dispatch("shakeCamera");
  };
</script>

<Hand position={[10, 5, 20]} rotY={Math.PI / 2} />
<Hand
  position={[-10, 5, -30]}
  rotY={-Math.PI / 2}
  on:shakeCamera={shakeCamera}
/>

<T.Mesh position={[0, 0, 0]} receiveShadow>
  <T.BoxGeometry args={[100, 0.1, 100]} />
  <T.MeshStandardMaterial color="hotpink" />
</T.Mesh>
