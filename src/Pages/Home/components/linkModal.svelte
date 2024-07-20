<script>
  import { createEventDispatcher } from "svelte"

  export let url = ""
  export let showModal = false
  const dispatch = createEventDispatcher()

  const handleInsert = () => {
    dispatch("linkInsert", { url })
  }

  const handleClose = () => {
    showModal = !showModal
  }
</script>

<div class:hidden={!showModal} class="modal-background flex items-center justify-center">
  <div class="modal-content">
    <label for="linkInput" class="block text-sm font-medium text-gray-700">Enter URL:</label>
    <input type="text" id="linkInput" class="mt-1 block w-full rounded-md border border-gray-300 p-2" value={url} />
    <div class="mt-4 flex justify-end">
      <button on:click={handleInsert} class="modal-button">Insert</button>
      <button on:click={handleClose} class="modal-button-cancel">Cancel</button>
    </div>
  </div>
</div>

<style lang="postcss">
  .modal-background {
    @apply fixed inset-0 bg-gray-600 bg-opacity-50;
  }

  .modal-content {
    @apply rounded bg-white p-6 shadow-lg;
  }

  .modal-button {
    @apply mr-2 rounded bg-blue-500 px-4 py-2 text-white;
  }

  .modal-button-cancel {
    @apply rounded bg-gray-500 px-4 py-2 text-white;
  }
</style>
