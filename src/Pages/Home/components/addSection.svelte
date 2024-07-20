<script>
  import Button from "./shared/button.svelte"
  import FileInput from "./shared/fileInput.svelte"
  let showMenu = false
  export let files
</script>

<div class="justify-left flex items-center gap-3">
  <!-- <button class="flex h-10 w-10 items-center justify-center rounded-md border border-[#a0a1a580] text-white disabled:cursor-not-allowed" on:click={() => (showMenu = !showMenu)}>
    <i class="fa-solid text-white" />
  </button> -->

  <Button iClass={showMenu ? "fa-xmark" : "fa-plus"} on:click={() => (showMenu = !showMenu)} />
  {#if showMenu}
    <FileInput iClass={"fa-image"} bind:files />
    <Button iClass={"fa-text"} />
    <Button iClass={"fa-video"} />
  {/if}
</div>
