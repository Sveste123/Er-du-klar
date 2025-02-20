<script lang="ts">
  import state from '/src/routes/FRESHuniversalState.json'
  import type * as THREE from 'three'
  import { Group, DirectionalLight, Object3D } from 'three'
  import { T, type Props, type Events, type Slots, forwardEventHandlers } from '@threlte/core'
  import { useGltf, Text3DGeometry } from '@threlte/extras'
  import { Theatre, SheetObject, Sequence } from '@threlte/theatre'
  import { onDestroy, onMount } from 'svelte';

  // Måldato: 7. april 2025 kl. 00:00:00
  const targetDate = new Date("2025-04-08T00:00:00").getTime();

  let countdown = getRemainingTime();

  function getRemainingTime() {
    const now = new Date().getTime();
    const timeLeft = targetDate - now;

    if (timeLeft <= 0) {
      return { days: 0, hours: 0, minutes: 0, seconds: 0 };
    }

    return {
      days: Math.floor(timeLeft / (1000 * 60 * 60 * 24)),
      hours: Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)),
      minutes: Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60)),
      seconds: Math.floor((timeLeft % (1000 * 60)) / 1000),
    };
  }

  const interval = setInterval(() => {
    countdown = getRemainingTime();
  }, 1000);

  onDestroy(() => clearInterval(interval));

  type $$Props = Props<THREE.Group>
  type $$Events = Events<THREE.Group>
  type $$Slots = Slots<THREE.Group> & { fallback: {}; error: { error: any } }

  export const ref = new Group()

  let bevelEnabled = true
  let bevelOffset = 0
  let bevelSegments = 1
  let bevelSize = 0
  let bevelThickness = 0.22
  let curveSegments = 12
  let depth = 0.65
  let size = 7.28
  let smooth = 1

  const component = forwardEventHandlers()
</script>

<Theatre config={{ state: state }} studio={{ hide: true, enabled: false }}>
  <Sequence autoplay>
    <!-- 💡 Directional Light -->
    <SheetObject key="3DText" props={{ scaleprop: 1, intensity: 0, posX: 0, posY: 0, posZ: 0 }} let:values>
      <!-- <T.DirectionalLight 
        intensity={values.intensity} 
        position={[values.posX, values.posY, values.posZ]}
        oncreate={(ref) => {
          ref.lookAt(0, 100, 0)
        }}
        castShadow
      /> -->
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8, 5, 0]}>
        <Text3DGeometry
          text="{`${countdown.days}d ${countdown.hours}t`}"
          {bevelEnabled}
          {bevelOffset}
          {bevelSegments}
          {bevelSize}
          {bevelThickness}
          {curveSegments}
          {depth}
          {size}
          {smooth}
        />
        <T.MeshStandardMaterial
          color="#fff000"
          toneMapped={false}
          metalness={1.0}
          roughness={0.1}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8, -5, 0]}>
        <Text3DGeometry
          text="{`${countdown.minutes}m ${countdown.seconds}s`}"
          {bevelEnabled}
          {bevelOffset}
          {bevelSegments}
          {bevelSize}
          {bevelThickness}
          {curveSegments}
          {depth}
          {size}
          {smooth}
        />
        <T.MeshStandardMaterial
          color="#fff000"
          toneMapped={false}
          metalness={1.0}
          roughness={0.1}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8, 5, -0.01]}>
        <Text3DGeometry
          text="{`${countdown.days}d ${countdown.hours}t`}"
          {bevelEnabled}
          {bevelOffset}
          {bevelSegments}
          bevelSize={0.09}
          {bevelThickness}
          {curveSegments}
          {depth}
          {size}
          {smooth}
        />
        <T.MeshStandardMaterial
          color="#e02e00"
          toneMapped={false}
          metalness={1.0}
          roughness={0.1}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8, -5, -0.01]}>
        <Text3DGeometry
          text="{`${countdown.minutes}m ${countdown.seconds}s`}"
          {bevelEnabled}
          {bevelOffset}
          {bevelSegments}
          bevelSize={0.09}
          {bevelThickness}
          {curveSegments}
          {depth}
          {size}
          {smooth}
        />
        <T.MeshStandardMaterial
          color="#e02e00"
          toneMapped={false}
          metalness={1.0}
          roughness={0.1}
        />
      </T.Mesh>

    </SheetObject>
  </Sequence>
</Theatre>
