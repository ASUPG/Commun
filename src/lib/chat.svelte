<script lang="ts">
  // Importing Importent Components
  import ExtendButton from "./extendbutton.svelte";
  // Declaring Importent Variables
  import {isOpen} from "./isOpen"
</script>

<!-- Side Grouping Menu -->
<ExtendButton bind:isOpen = {$isOpen} />
{#if $isOpen == true}
<div class="sidenav h-screen w-1/3 bg-slate-950" />
{:else}
<div class="sidenav h-screen w-0 bg-slate-950" />
{/if}
<style lang="scss">
  .sidenav {
    position: absolute;
    top: 0;
    z-index: 0;
  }
  // reponsiveness
  @media only screen and (max-width: 900px) {
    .sidenav {
      left: 48px;
      width: 300px !important;
      display: none;
    }
  }
</style>
