<script>
  import LinkModal from "./components/linkModal.svelte"

  let showModal = false
  let url = ""
  let isTextSelected = false

  const formatText = (command, value = null) => {
    document.execCommand(command, false, value)
  }

  let savedRange = null

  const openLinkModal = () => {
    const selection = window.getSelection()
    if (selection.rangeCount > 0) {
      savedRange = selection.getRangeAt(0)
    }
    url = savedRange ? savedRange.toString() : ""
    showModal = !showModal
  }

  const insertLink = (event) => {
    url = event.detail.url.trim()

    if (url && savedRange) {
      const selection = window.getSelection()
      if (savedRange.toString().trim() !== "") {
        const anchor = document.createElement("a")
        anchor.href = url
        anchor.target = "_blank"

        const fragment = savedRange.extractContents()
        anchor.appendChild(fragment)
        savedRange.insertNode(anchor)

        // Clear and restore selection
        selection.removeAllRanges()
        selection.addRange(savedRange)
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

  :global([contenteditable="true"] a) {
    color: rgb(159, 159, 218);
    text-decoration: underline;
    border-bottom: 2px solid rgb(36, 36, 80);
  }

  :global([contenteditable="true"] a:hover) {
    text-decoration: none;
    border-bottom: 2px solid darkblue;
  }
</style>
