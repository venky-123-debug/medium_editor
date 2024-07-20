<script>
  import LinkModal from "./components/linkModal.svelte"
  import Button from "./shared/button.svelte"

  let showModal = false
  let url = ""
  let isTextSelected = false

  const formatText = (command, value = null) => {
    document.execCommand(command, false, value)
  }

  let isBoldActive = false
  let isItalicActive = false
  let isUnderlineActive = false
  let isLinkActive = false

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

  // const checkTextSelection = () => {
  //   isTextSelected = window.getSelection().toString().length > 0
  // }
  const checkTextSelection = () => {
    isTextSelected = window.getSelection().toString().length > 0
    updateActiveStates() // Update active states on selection change
  }

  const updateActiveStates = () => {
    const selection = window.getSelection()
    if (selection.rangeCount > 0) {
      const range = selection.getRangeAt(0)
      const parent = range.commonAncestorContainer

      // Check if text is bold, italic, or underline
      isBoldActive = document.queryCommandState("bold")
      isItalicActive = document.queryCommandState("italic")
      isUnderlineActive = document.queryCommandState("underline")
      // const isLink = (node) => node.nodeName === "A" || (node.parentNode && isLink(node.parentNode))
      // isLinkActive = isLink(range.commonAncestorContainer)
    }
  }
</script>

<div class="flex min-h-screen items-center justify-center bg-gray-900">
  <div class="w-full max-w-2xl">
    <div class="toolbar flex space-x-2">
      <Button iClass="fa-bold" isSelected={isTextSelected} active={isBoldActive} on:click={() => formatText("bold")} />
      <Button iClass="fa-italic" isSelected={isTextSelected} active={isItalicActive} on:click={() => formatText("italic")} />
      <Button iClass="fa-underline" isSelected={isTextSelected} active={isUnderlineActive} on:click={() => formatText("underline")} />
      <!-- Uncomment if needed -->
      <!-- <Button 
        isSelected={isTextSelected} 
        on:click={() => formatText("insertOrderedList")} 
      />
      <Button 
        isSelected={isTextSelected} 
        on:click={() => formatText("insertUnorderedList")} 
      /> -->
      <Button iClass="fa-link" isSelected={isTextSelected} on:click={openLinkModal} />
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
