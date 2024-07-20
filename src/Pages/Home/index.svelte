<script>
  import AddSection from "./components/addSection.svelte"
  import LinkModal from "./components/linkModal.svelte"
  import Button from "./shared/toolBarButton.svelte"

  let showModal = false
  let url = ""
  let isTextSelected = false
  let content = ""
  // let files
  let files = []
  let imageUrls = []
  $: {
    console.log("index files", files)
  }
  let filesArray = []

  const formatText = (command, value = null) => {
    document.execCommand(command, false, value)
  }

  let isBoldActive = false
  let isItalicActive = false
  let isUnderlineActive = false
  // let isLinkActive = false

  let savedRange = null

  const openLinkModal = () => {
    const selection = window.getSelection()
    if (selection.rangeCount > 0) {
      savedRange = selection.getRangeAt(0)
    }
    url = savedRange ? savedRange.toString() : ""
    showModal = !showModal
  }

  const insertLink = (e) => {
    url = e.detail.url.trim()

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
  let showMenu = false
  let videLinkModal = false
  let videoDetails = null
  let videoUrl = ""
  const handleFileUpload = (e) => {
    const uploadedFiles = e.detail.files
    imageUrls = uploadedFiles.map((file) => URL.createObjectURL(file))
    showMenu = !showMenu
  }
  const fetchVideoDetails = (url) => {
    const videoId = extractVideoId(url)
    const thumbnailUrl = getThumbnailUrl(videoId, "hqdefault")
    // Note: Extracting title and duration requires API key, so we can only fetch thumbnail here.

    // Set video details for display
    videoDetails = {
      title: "Unknown Title", // Default or fetch title via API if available
      thumbnail: thumbnailUrl,
      duration: "Unknown Duration", // Default or fetch duration via API if available
    }

    console.log({ videoDetails })
  }

  const extractVideoId = (url) => {
    const urlParams = new URLSearchParams(new URL(url).search)
    return urlParams.get("v") || url.split("/").pop()
  }

  const getThumbnailUrl = (videoId, quality = "hqdefault") => {
    return `https://img.youtube.com/vi/${videoId}/${quality}.jpg`
  }

  const openVideoLinkModal = () => {
    videLinkModal = !videLinkModal
  }

  const handleVideoLinkSubmit = (e) => {
    videoUrl = e.detail.url
    fetchVideoDetails(videoUrl)
    openVideoLinkModal() // Close the modal after fetching details
  }
</script>

<div class="flex min-h-screen items-center justify-center bg-gray-900">
  <div class="flex w-full max-w-2xl flex-col gap-3">
    <AddSection bind:files on:fileUpload={handleFileUpload} bind:showMenu on:click={() => (videLinkModal = true)} />
    {#if imageUrls.length > 0}
      <div class="mt-4">
        {#each imageUrls as imageUrl}
          <!-- svelte-ignore a11y-img-redundant-alt -->
          <img src={imageUrl} alt="Uploaded Image" class="mb-2 aspect-auto h-auto w-full" />
        {/each}
      </div>
    {/if}
    {#if isTextSelected || isBoldActive || isItalicActive || isUnderlineActive}
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
    {/if}
    <div
      class="min-h-[200px] w-full rounded border border-gray-500 bg-transparent p-4 text-white shadow placeholder:text-base placeholder:text-gray-400 focus:border-blue-500 focus:outline-none"
      contenteditable="true"
      placeholder="Type something ..."
      on:input={checkTextSelection}
      on:mouseup={checkTextSelection}
      on:keyup={checkTextSelection}
    >
      {content}
    </div>
  </div>

  <LinkModal on:linkInsert={insertLink} bind:url bind:showModal />
  <!-- {#if videLinkModal} -->
  <LinkModal url={videoUrl} showModal={videLinkModal} on:linkInsert={handleVideoLinkSubmit} />
  <!-- {/if} -->
</div>

<style lang="postcss">
  :global([contenteditable="true"] a) {
    color: rgb(159, 159, 218);
    /* text-decoration: underline; */
    border-bottom: 2px solid rgb(36, 36, 80);
  }

  :global([contenteditable="true"] a:hover) {
    text-decoration: none;
    border-bottom: 2px solid darkblue;
  }
</style>
