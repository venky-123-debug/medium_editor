<script>
  import { createEventDispatcher } from "svelte"

  const dispatch = createEventDispatcher()

  export let showModal = false

  const insertLink = () => {
    dispatch("linkInsert")
  }
  const closeLinkModal = () => {
    dispatch("closeModal")
  }
</script>

<div class:hidden={!showModal} class="fixed inset-0 hidden items-center justify-center bg-gray-600 bg-opacity-50">
  <div class="rounded bg-white p-6 shadow-lg">
    <label for="linkInput" class="block text-sm font-medium text-gray-700">Enter URL:</label>
    <input type="text" id="linkInput" class="mt-1 block w-full rounded-md border border-gray-300 p-2" />
    <div class="mt-4 flex justify-end">
      <button on:click={insertLink} class="mr-2 rounded bg-blue-500 px-4 py-2 text-white">Insert</button>
      <button on:click={closeLinkModal} class="rounded bg-gray-500 px-4 py-2 text-white">Cancel</button>
    </div>
  </div>
</div>
