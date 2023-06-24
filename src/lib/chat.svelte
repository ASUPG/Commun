<script lang="ts">
  // Importing Importent Components
  import ExtendButton from "./extendbutton.svelte";
  // Declaring Importent Variables
  let isOpen = false;
  let oldIsOpen = isOpen;
  //Adding logic to open button
  setInterval(() => {
    if (isOpen != oldIsOpen) {
      if (isOpen) {
        document.getElementById("sidenav").style.transform = "scaleX(1)";
      } else {
        document.getElementById("sidenav").style.transform = "scaleX(0)";
      }
      oldIsOpen = isOpen;
    }
    
  }, 100);
</script>

<chatwindows style="display: flex;">
  <!-- Side Grouping Menu -->
  <ExtendButton bind:isOpen />
  <div id="sidenav" class="sidenav h-screen w-1/3 bg-slate-950" />
</chatwindows>

<style lang="scss">
  .sidenav {
    position: absolute;
    top: 0;
    z-index: 0;
    transition: 2s all;
    align-self: flex-start;
  }
  // reponsiveness
  @media only screen and (max-width: 900px) {
    .sidenav {
      left: 48px;
      width: 300px !important;
      transform: scaleX(0);
    }
  }
</style>
