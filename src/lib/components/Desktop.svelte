<script lang="ts">
  import DesktopIcon from "./DesktopIcon.svelte";
  import { appState, monitorRevealed } from "$lib/stores/appState";
  import {
    FileMd,
    Folder,
    FolderOpen,
    Trash,
    Gear,
    MusicNotes,
    Notebook,
  } from "phosphor-svelte";

  let projectsOpen = $state(false);
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

  function openSettings() {
    // TODO: open settings window
    console.log("Settings clicked");
  }

  function openProjects() {
    projectsOpen = !projectsOpen;
    // TODO: open projects window
  }

  function openTrash() {
    // TODO: open trash
    console.log("Trash clicked");
  }

  function openMusic() {
    // TODO: open music player
    console.log("Music clicked");
  }

  function openNotebook() {
    // TODO: open notebook/blog
    console.log("Notebook clicked");
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
    <DesktopIcon
      label="Projects"
      icon={projectsOpen ? FolderOpen : Folder}
      onclick={openProjects}
    />
    <DesktopIcon label="Notebook" icon={Notebook} onclick={openNotebook} />
    <DesktopIcon label="Music" icon={MusicNotes} onclick={openMusic} />
    <DesktopIcon label="Settings" icon={Gear} onclick={openSettings} />
    <DesktopIcon label="Trash" icon={Trash} onclick={openTrash} />
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
