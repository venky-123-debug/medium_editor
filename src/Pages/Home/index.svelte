<script>
  import LinkModal from "./modules/linkModal.svelte"

  let showModal = false
  function formatText(command, value = null) {
    document.execCommand(command, false, value)
  }

  function openLinkModal() {
    showModal = !showModal
  }

  function insertLink() {
    const url = document.getElementById("linkInput").value
    if (url) {
      formatText("createLink", url)
    }
    openLinkModal()
  }

  $: console.log({ showModal })
</script>

<div class="flex min-h-screen items-center justify-center bg-gray-900">
  <div class="w-full max-w-2xl">
    <div class="toolbar flex space-x-2">
      <button on:click={() => formatText("bold")}>Bold</button>
      <button on:click={() => formatText("italic")}>Italic</button>
      <button on:click={() => formatText("underline")}>Underline</button>
      <button on:click={() => formatText("insertOrderedList")}>OL</button>
      <button on:click={() => formatText("insertUnorderedList")}>UL</button>
      <button on:click={() => openLinkModal()}>Link</button>
    </div>
    <div class="mt-2 min-h-[200px] w-full rounded border border-gray-500 bg-transparent p-4 text-white shadow focus:border-blue-500 focus:outline-none" contenteditable="true" />
  </div>

  <LinkModal on:linkInsert={insertLink} on:closeModal={openLinkModal} {showModal} />
</div>

<style lang="postcss">
  .toolbar button {
    @apply m-1 rounded bg-gray-300 p-2;
  }
  .toolbar button:hover {
    @apply bg-gray-400;
  }
</style>
