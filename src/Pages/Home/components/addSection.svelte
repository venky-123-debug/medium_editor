<script>
  import Button from "./shared/button.svelte"
  import FileInput from "./shared/fileInput.svelte"
  export let showMenu = false
  export let files

  const handleMenu = () => {
    showMenu = !showMenu
  }
</script>

<div class="justify-left flex items-center gap-3">
  <Button iClass={showMenu ? "fa-xmark" : "fa-plus"} on:click={handleMenu} />
  {#if showMenu}
    <FileInput iClass={"fa-image"} bind:files on:fileUpload />
    <Button iClass={"fa-text"} />
    <Button iClass={"fa-video"} />
  {/if}
</div>
