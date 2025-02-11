<script lang="ts">
  import { T, useTask } from '@threlte/core'
  import { Align, ContactShadows, Float, Grid, OrbitControls, Gizmo, TrackballControls, Mask, useMask, MeshLineGeometry, MeshLineMaterial } from '@threlte/extras'
  import * as Extra from '@threlte/extras'
	import { BoxGeometry, Group, MeshBasicMaterial, DirectionalLight, Object3D, AmbientLight } from 'three';
  import { writable } from 'svelte/store';
  // import * as Utils from 'three/src/math/MathUtils'
  import { Environment, interactivity, Text, useCursor, Text3DGeometry } from '@threlte/extras'
	import Spark from './Spark.svelte';
  import { onDestroy, onMount } from 'svelte';
	import DirectionalLight1 from './DirectionalLight1.svelte';

  // export let text: string
  export let bevelEnabled: boolean
  export let bevelOffset: number
  export let bevelSegments: number
  export let bevelSize: number
  export let bevelThickness: number
  export let curveSegments: number
  export let depth: number
  export let size: number
  export let smooth: number



  // // Lysets target-posisjon (default peker mot origo)
  // export let targetX = writable(0);
  // export let targetY = writable(0);
  // export let targetZ = writable(0);

  // let light;
  // let targetObject = new Object3D()

  // // Når komponenten er mountet, settes target på lyset
  // onMount(() => {
  //   light.target = targetObject
  // })

  // // Oppdater posisjonen til target dynamisk
  // $: targetObject.position.set($targetX, $targetY, $targetZ)


  
  // Reactive store for media query
  const isSmallScreen = writable(false)
  const mediaQuery = window.matchMedia('(max-width: 768px)')
  const updateScreenSize = () => {
    isSmallScreen.set(mediaQuery.matches)
  }
  mediaQuery.addEventListener('change', updateScreenSize)
  updateScreenSize() // Initialize

  $: cameraProps = $isSmallScreen
    ? { fov: 140, position: [0, 0, 20] } // Small screen
    : { fov: 90, position: [0, 0, 20] } // Large screen

  let currentTime = new Date()
  const updateTime = () => {
    currentTime = new Date()
  }
  const interval = setInterval(updateTime, 1000)
  onDestroy(() => clearInterval(interval))
</script>

<Float
  rotationIntensity={0.5}
  rotationSpeed={2}
>
  <T.PerspectiveCamera
    makeDefault
    fov={cameraProps.fov}
    position={cameraProps.position}
  >
    <OrbitControls
      enableZoom={false}
      enableDamping={true}
      enablePan={false}
      autoRotate={false}
      autoRotateSpeed={0.1}
      target={[0, 0, 0]}
      maxPolarAngle={2}
      minPolarAngle={1}
    />
  </T.PerspectiveCamera>
</Float>


<!-- <T.DirectionalLight
  bind:light={light}
  intensity={1}
  position={[5, 5, 15]}
  castShadow
/> -->

<!-- Legg til targetObject i scenen slik at lyset peker mot det
<T.Object3D 
  bind:object3d={targetObject} 
  position={[$targetX, $targetY, $targetZ]}
/> -->



<T.AmbientLight
  intensity={1}
  position={[5, 5, 15]}
  castShadow
/>

<ContactShadows
  scale={10}
  blur={1}
  far={0.5}
  opacity={0.5}
/>

<T.Mesh scale.z={depth / 20} position={[-19.6, 5, 0]}>
  <Text3DGeometry
    text="{currentTime.toLocaleTimeString()}"
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
    color="#FD3FFF"
    toneMapped={false}
    metalness={1.0}
    roughness={0.1}
  />
</T.Mesh>

<T.Mesh scale.z={depth / 20} position={[-19.6, -5, 0]}>
  <Text3DGeometry
    text="{currentTime.toLocaleDateString()}"
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
    color="#FD3FFF"
    toneMapped={false}
    metalness={1.0}
    roughness={0.1}
  />
</T.Mesh>

<!-- <Spark
  scale={0.01}
  position={[0, 0, 0]}
/> -->

<DirectionalLight1/>