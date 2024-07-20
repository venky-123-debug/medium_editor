<script>
  import { createEventDispatcher } from "svelte"

  const dispatch = createEventDispatcher()
  export let url = ""
  export let showModal = false

  const handleInsert = () => {
    dispatch("linkInsert", { url })
  }

  const handleClose = () => {
    showModal = !showModal
  }
</script>

<div class:hidden={!showModal} class=" fixed inset-0 flex items-center justify-center bg-gray-600 bg-opacity-50 backdrop-blur-md">
  <div class="flex min-w-[50%] flex-col gap-3 rounded-md bg-black p-6 shadow-lg">
    <label for="linkInput" class="block text-sm font-medium text-gray-400">Enter URL:</label>
    <input type="text" id="linkInput" class="mt-1 block w-full rounded-md border border-gray-300 bg-transparent p-2 text-white" bind:value={url} />
    <div class="mt-4 flex justify-end">
      <button on:click={handleInsert} class="mr-2 rounded-md bg-blue-500 px-4 py-2 text-white">Insert</button>
      <button on:click={handleClose} class=" rounded-md bg-gray-500 px-4 py-2 text-white">Cancel</button>
    </div>
  </div>
</div>
