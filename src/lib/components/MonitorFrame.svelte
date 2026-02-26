<script lang="ts">
  import { Power } from "phosphor-svelte";
  import { monitorRevealed } from "$lib/stores/appState";

  let { children } = $props();
</script>

<div class="monitor" class:revealed={$monitorRevealed}>
  <div class="bezel-inner">
    <div class="screen">
      {@render children()}
    </div>
  </div>
  <button
    class="power-button"
    class:visible={$monitorRevealed}
    aria-label="Power"
  >
    <span class="power-icon">
      <Power size={14} weight="bold" />
    </span>
  </button>
</div>

<style>
  .monitor {
    position: fixed;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: var(--bezel-color);
  }

  .bezel-inner {
    width: 100%;
    flex: 1;
    display: flex;
    justify-content: center;
    padding: 0;
    transition: padding 0.35s ease-out;
  }

  .monitor.revealed .bezel-inner {
    padding-left: var(--bezel-x);
    padding-right: var(--bezel-x);
    padding-top: var(--bezel-top);
  }

  .screen {
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 100%;
    border-radius: 0;
    transition: border-radius 0.35s ease-out;
  }

  .monitor.revealed .screen {
    border-radius: var(--screen-radius);
  }

  /* Bottom bezel area */
  .monitor::after {
    content: "";
    width: 100%;
    height: 0;
    flex-shrink: 0;
    transition: height 0.35s ease-out;
  }

  .monitor.revealed::after {
    height: var(--bezel-bottom);
  }

  /* Power button — 3D beveled circle */
  .power-button {
    position: absolute;
    bottom: calc(var(--bezel-bottom) / 2);
    right: calc(var(--bezel-x) + 8px);
    transform: translateY(50%);
    width: 28px;
    height: 28px;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;

    /* 3D bevel: light top-left edge, dark bottom-right edge */
    background: radial-gradient(
        circle at 35% 35%,
        oklch(100% 0 0 / 0.15),
        transparent 60%
      ),
      linear-gradient(
        135deg,
        oklch(100% 0 0 / 0.2) 0%,
        transparent 50%,
        oklch(0% 0 0 / 0.3) 100%
      ),
      oklch(35% 0.01 70);
    box-shadow:
      /* outer raised edge */
      1px 2px 3px oklch(0% 0 0 / 0.4),
      -1px -1px 2px oklch(100% 0 0 / 0.08),
      /* inset rim highlight */ inset 0 1px 1px oklch(100% 0 0 / 0.15),
      inset 0 -1px 1px oklch(0% 0 0 / 0.2);

    opacity: 0;
    pointer-events: none;
    transition:
      opacity 0.35s ease-out 0.15s,
      box-shadow 0.15s ease,
      transform 0.1s ease;
  }

  .power-button.visible {
    opacity: 1;
    pointer-events: auto;
  }

  .power-button:hover {
    box-shadow:
      1px 2px 4px oklch(0% 0 0 / 0.5),
      -1px -1px 2px oklch(100% 0 0 / 0.1),
      inset 0 1px 1px oklch(100% 0 0 / 0.2),
      inset 0 -1px 1px oklch(0% 0 0 / 0.25),
      0 0 8px var(--power-glow);
  }

  .power-button:active {
    transform: translateY(calc(50% + 1px));
    box-shadow:
      0 1px 1px oklch(0% 0 0 / 0.4),
      inset 0 1px 2px oklch(0% 0 0 / 0.25),
      inset 0 -1px 1px oklch(100% 0 0 / 0.1);
  }

  .power-icon {
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--power-color);
    filter: drop-shadow(0 0 3px var(--power-glow));
    transition: filter 0.15s ease;
  }

  .power-button:hover .power-icon {
    filter: drop-shadow(0 0 6px var(--power-glow));
  }
</style>
