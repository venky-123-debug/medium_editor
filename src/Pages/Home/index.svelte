<script>
  import LinkModal from "./components/linkModal.svelte"

  let showModal = false
  let url = ""
  let isTextSelected = false

  const formatText = (command, value = null) => {
    document.execCommand(command, false, value)
  }

  const openLinkModal = () => {
    showModal = !showModal
  }

  const insertLink = (event) => {
    url = event.detail.url
    if (url) {
      const selection = window.getSelection()
      if (selection.rangeCount > 0 && !selection.isCollapsed) {
        const range = selection.getRangeAt(0)
        const selectedText = range.toString()
        const anchor = document.createElement("a")
        anchor.href = url
        anchor.target = "_blank"
        anchor.textContent = selectedText
        range.deleteContents()
        range.insertNode(anchor)
        selection.removeAllRanges()
        selection.addRange(range)
      }
    }
    openLinkModal()
  }

  const checkTextSelection = () => {
    isTextSelected = window.getSelection().toString().length > 0
  }
</script>

<div class="flex min-h-screen items-center justify-center bg-gray-900">
  <div class="w-full max-w-2xl">
    <div class="toolbar flex space-x-2">
      <button on:click={() => formatText("bold")} disabled={!isTextSelected}>Bold</button>
      <button on:click={() => formatText("italic")} disabled={!isTextSelected}>Italic</button>
      <button on:click={() => formatText("underline")} disabled={!isTextSelected}>Underline</button>
      <!-- <button on:click={() => formatText("insertOrderedList")} disabled={!isTextSelected}>OL</button>
      <button on:click={() => formatText("insertUnorderedList")} disabled={!isTextSelected}>UL</button> -->
      <button on:click={openLinkModal} disabled={!isTextSelected}>Link</button>
    </div>
    <div
      class="mt-2 min-h-[200px] w-full rounded border border-gray-500 bg-transparent p-4 text-white shadow focus:border-blue-500 focus:outline-none"
      contenteditable="true"
      on:input={checkTextSelection}
      on:mouseup={checkTextSelection}
      on:keyup={checkTextSelection}
    />
  </div>

  <LinkModal on:linkInsert={insertLink} bind:url bind:showModal />
</div>

<style lang="postcss">
  .toolbar button {
    @apply m-1 rounded bg-gray-300 p-2;
  }
  .toolbar button:hover {
    @apply bg-gray-400;
  }
  .toolbar button:disabled {
    @apply cursor-not-allowed bg-gray-400;
  }
</style>
