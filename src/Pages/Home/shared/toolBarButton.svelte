<!-- ToolbarButton.svelte -->
<script>
  export let isSelected = false
  export let iClass = ""
  export let active = ""
</script>

<button class="flex h-10 w-10 items-center justify-center rounded-md border border-[#a0a1a580] text-white disabled:cursor-not-allowed {active ? `bg-blue-500` : ``}" on:click disabled={!isSelected}>
  <i class="fa-solid {iClass} text-lg text-white" />
</button>
