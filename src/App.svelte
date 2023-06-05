<script lang="ts">
  // Importing neccasry components
  import Header from "./lib/header.svelte"
  import Chat from "./lib/chat.svelte"
</script>

<!-- Header -->
<Header />
<!-- Main Body -->
<main class="h-screen w-screen">
  <!-- Chat Window -->
  <Chat />
</main>

<style lang="scss">
  main {
    background-color: #0d0d0d;
  }
</style>
