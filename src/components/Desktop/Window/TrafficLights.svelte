<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import CloseIcon from '__/components/SVG/traffic-lights/CloseSVG.svelte';
  import GreenLight from '__/components/SVG/traffic-lights/GreenLight.svelte';
  import MinimizeSvg from '__/components/SVG/traffic-lights/MinimizeSVG.svelte';
  import { appsConfig } from '__/configs/apps/apps-config';
  import type { AppID } from '__/stores/apps.store';
  import { activeApp, openApps } from '__/stores/apps.store';

  export let appID: AppID;

  const dispatch = createEventDispatcher();

  function closeApp() {
    $openApps[appID] = false;
  }

  function greenLightAction() {
    dispatch('maximize-click');
  }
</script>

<div class="container" class:unfocused={$activeApp !== appID}>
  <button class="close-light" on:click={closeApp}> <CloseIcon /> </button>
  <button class="minimize-light"> <MinimizeSvg /> </button>
  <button class="stretch-light" on:click={greenLightAction}>
    <GreenLight expandable={appsConfig[appID].expandable} />
  </button>
</div>

<style lang="scss">
  .container {
    --button-size: 0.8rem;

    // pointer-events: none;

    display: grid;
    grid-template-columns: repeat(3, var(--button-size));
    align-items: center;
    gap: 0.6rem;

    height: 100%;

    &.unfocused button {
      --bgcolor: #b6b6b7;
      --border-color: hsla(var(--app-color-dark-hsl), 0.5);
    }

    :global(svg) {
      visibility: hidden;
    }

    &:hover :global(svg) {
      visibility: visible;
    }
  }

  button {
    height: var(--button-size);
    width: var(--button-size);

    // pointer-events: initial;

    border-radius: 50%;

    background-color: var(--bgcolor);
    box-shadow: 0 0 0 0.5px var(--border-color);
  }

  .close-light {
    --bgcolor: #ff5f56;
    --border-color: #e0443e;
  }

  .stretch-light {
    --bgcolor: #27c93f;
    --border-color: #1aab29;
    transform: rotate(90deg);
  }

  .minimize-light {
    --bgcolor: #ffbd2e;
    --border-color: #dea123;
  }
</style>
