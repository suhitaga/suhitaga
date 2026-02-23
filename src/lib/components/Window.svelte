<script lang="ts">
  import { X, Minus } from "phosphor-svelte";
  import "./Window.css";

  export let title: string = "Window";
  export let isMinimized: boolean = false;
  export let showClose: boolean = false;
  export let zIndex: number = 1;
  export let onminimize: (() => void) | undefined = undefined;
  export let onclose: (() => void) | undefined = undefined;
  export let onfocus: (() => void) | undefined = undefined;

  function handleMinimize() {
    onminimize?.();
  }

  function handleClose() {
    onclose?.();
  }

  function handleFocus() {
    onfocus?.();
  }
</script>

<div
  class="window"
  class:minimized={isMinimized}
  style:z-index={zIndex}
  on:mousedown={handleFocus}
  role="dialog"
  aria-label={title}
  tabindex="-1"
>
  <header class="title-bar">
    <span class="title">{title}</span>
    <div class="controls">
      {#if showClose}
        <button
          class="control close"
          on:click={handleClose}
          aria-label="Close window"
        >
          <X size={16} weight="bold" />
        </button>
      {/if}
      <button
        class="control minimize"
        on:click={handleMinimize}
        aria-label="Minimize window"
      >
        <Minus size={16} weight="bold" />
      </button>
    </div>
  </header>
  <div class="window-content">
    <slot />
  </div>
</div>

<style>
  .window {
    position: fixed;
    inset: 0;
    background: var(--bg-primary);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    transform-origin: var(--fold-target-x, 68px) var(--fold-target-y, 80px);
    transition: transform 0.35s ease-out, opacity 0.3s ease-out;
  }

  .window.minimized {
    transform: scale(0);
    opacity: 0;
    pointer-events: none;
  }
</style>
