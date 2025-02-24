<script lang="ts">
  import App from "../lib/components/App.svelte"
  import { List, type ListOptions } from 'svelte-tweakpane-ui';
  import { onDestroy, onMount } from 'svelte';

  let showIndicator = true; // Bruker en tilstand for å styre synlighet
  function showIndicatorFalse(e: Event){
        showIndicator = false;
    }

  // onMount(() => {
  //   setTimeout(() => {
  //     window.location.href = "//www.festivalente.no";
  //   }, 12500); // 15000ms = 15 sekunder
  // });

  // function handleRedirect() {
  //   console.log("Redirect triggered")
  //   window.location.href = "//www.festivalente.no";
  // }
</script>

<div class="app" on:pointerdown={showIndicatorFalse}>
  <App />
</div>

<!-- <div class="overlay">
  <img src="/SOOTSN.png" alt="Overlay" />
</div> -->

<!-- {#if showIndicator}
  <img alt="finger" class="scrollhand" src="/scrollhand.png"/>
{/if} -->