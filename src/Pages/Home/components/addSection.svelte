<script>
  let showMenu = false
</script>

<div class="justify-left flex items-center gap-3">
  <button class="flex h-10 w-10 items-center justify-center rounded-md border border-[#a0a1a580] text-white disabled:cursor-not-allowed" on:click={() => (showMenu = !showMenu)}>
    <i class="fa-solid {showMenu ? 'fa-xmark' : 'fa-plus'} text-white" />
  </button>
</div>
