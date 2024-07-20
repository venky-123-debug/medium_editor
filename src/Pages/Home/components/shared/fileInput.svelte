<script>
  import { createEventDispatcher } from "svelte"
  const dispatch = createEventDispatcher()
  export let iClass = ""
  let fileInput
  export let files

  // function handleFileChange(event) {
  //   files = event.target.files
  //   // Handle the file(s) as needed
  //   console.log(files)
  // }
  function handleFileChange(event) {
    const selectedFiles = event.target.files
    if (selectedFiles.length > 0) {
      files = Array.from(selectedFiles)
      dispatch("fileUpload", { files })
    }
  }
</script>

<div title="insert image" class="flex h-10 w-10 items-center justify-center rounded-md border border-[#a0a1a580] text-white">
  <input type="file" accept=".png,.jpeg,.jpg" bind:files bind:this={fileInput} on:change={handleFileChange} class="hidden" />
  <button on:click={() => fileInput.click()} class="flex h-full w-full items-center justify-center rounded-md text-white disabled:cursor-not-allowed">
    <i class="fa-solid {iClass} text-lg text-white" />
  </button>
</div>
