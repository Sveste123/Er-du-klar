<script lang="ts">
  import state from '/src/routes/FRESHuniversalState.json'
  import type * as THREE from 'three'
  import { Group, DirectionalLight, Object3D } from 'three'
  import { T, type Props, type Events, type Slots, forwardEventHandlers } from '@threlte/core'
  import { useGltf, Text3DGeometry } from '@threlte/extras'
  import { Theatre, SheetObject, Sequence } from '@threlte/theatre'
  import { onDestroy, onMount } from 'svelte';
	import { text } from '@sveltejs/kit';

  // Måldato: 8. april 2025 kl. 00:00:00
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
  let bevelThickness = 0.2
  let curveSegments = 3
  let depth = 0.6
  let size = 7.3
  let smooth = 1

  const component = forwardEventHandlers()

  // export let onRedirect = () => {};

  function onRedirect() {
    console.log("Redirect triggered")
    window.location.href = "//www.festivalente-2025.vercel.app";
  }

</script>

<Theatre config={{ state: state }} studio={{ hide: true, enabled: false }}>
  <Sequence autoplay>
    <SheetObject key="Countdown3D" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0 }} let:values>
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
          visible={values.visible}
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
          visible={values.visible}
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
          visible={values.visible}
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>


    <SheetObject key="Dager" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0 }} let:values>
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), 0]}>
        <Text3DGeometry
          text="{`${countdown.days} Dager`}"
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
          visible={values.visible}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), -0.01]}>
        <Text3DGeometry
          text="{`${countdown.days} Dager`}"
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>

    <SheetObject key="Timer" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0 }} let:values>
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), 0]}>
        <Text3DGeometry
          text="{`${countdown.hours} Timer`}"
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
          visible={values.visible}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), -0.01]}>
        <Text3DGeometry
          text="{`${countdown.hours} Timer`}"
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>

    <SheetObject key="Minutter" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0 }} let:values>
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), 0]}>
        <Text3DGeometry
          text="{`${countdown.minutes} Minutter`}"
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
          visible={values.visible}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), -0.01]}>
        <Text3DGeometry
          text="{`${countdown.minutes} Minutter`}"
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>

    <SheetObject key="Sekunder" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0 }} let:values>
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), 0]}>
        <Text3DGeometry
          text="{`${countdown.seconds} Sekunder`}"
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
          visible={values.visible}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), -0.01]}>
        <Text3DGeometry
          text="{`${countdown.seconds} Sekunder`}"
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>

    <SheetObject key="Custom text" props={{ scaleprop: 1, visible: false, posX: 0, posY: 0, posZ: 0, text: '""' }} let:values>
      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), 0]}>
        <Text3DGeometry
          text={values.text}
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
          visible={values.visible}
        />
      </T.Mesh>

      <T.Mesh scale={[values.scaleprop, values.scaleprop, depth / 20]} position={[-18.8 + (values.posX), 5 + (values.posY), -0.01]}>
        <Text3DGeometry
          text={values.text}
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
          visible={values.visible}
        />
      </T.Mesh>
    </SheetObject>

    <SheetObject key="RedirectTrigger" props={{ redirect: false }} let:values>
      {#if values.redirect}
        <!-- Når redirect blir true, kall onRedirect -->
        <svelte:component this={onRedirect()} />
      {/if}
    </SheetObject>

  </Sequence>
</Theatre>
