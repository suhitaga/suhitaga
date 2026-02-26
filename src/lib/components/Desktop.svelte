<script lang="ts">
  import DesktopIcon from "./DesktopIcon.svelte";
  import { appState, monitorRevealed } from "$lib/stores/appState";
  import { FileMd } from "phosphor-svelte";

  let resumeIconEl: HTMLElement | null = $state(null);
  let desktopEl: HTMLElement | null = $state(null);

  function updateIconPosition() {
    if (resumeIconEl && desktopEl) {
      const iconRect = resumeIconEl.getBoundingClientRect();
      const containerRect = desktopEl.getBoundingClientRect();
      const centerX = iconRect.left + iconRect.width / 2 - containerRect.left;
      const centerY = iconRect.top + iconRect.height / 2 - containerRect.top;
      document.documentElement.style.setProperty(
        "--fold-target-x",
        `${centerX}px`,
      );
      document.documentElement.style.setProperty(
        "--fold-target-y",
        `${centerY}px`,
      );
    }
  }

  $effect(() => {
    if (resumeIconEl && desktopEl) {
      updateIconPosition();
      window.addEventListener("resize", updateIconPosition);
      return () => window.removeEventListener("resize", updateIconPosition);
    }
  });

  $effect(() => {
    if ($monitorRevealed && resumeIconEl && desktopEl) {
      const timer = setTimeout(updateIconPosition, 400);
      return () => clearTimeout(timer);
    }
  });

  function openResume() {
    appState.goToResume();
  }


</script>

<div class="desktop" bind:this={desktopEl}>
  <div class="icon-grid">
    <DesktopIcon
      label="resume.md"
      icon={FileMd}
      onclick={openResume}
      bind={(el) => (resumeIconEl = el)}
    />
  </div>
</div>

<style>
  .desktop {
    position: absolute;
    inset: 0;
    background: var(--bg-primary);
    padding: 24px;
    overflow: hidden;
  }

  .icon-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, 88px);
    grid-auto-rows: 100px;
    gap: 8px;
    align-content: start;
  }
</style>
