<script lang="ts">
  import { initializeApp } from "firebase/app";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { config } from "./../fbaseconfig";
  let app = initializeApp(config);
  let db = getFirestore(app);
</script>

<div id="wrapper">
  <!-- Header -->
  <header>
    <nav>
      <h1 class="brand-heading">Friends Forever</h1>
      <div class="themechanger">
        <i class="fa-regular fa-circle-half-stroke" style="color: #ffffff;" />
      </div>
    </nav>
  </header>
</div>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Vibur&display=swap');
  @import url("./fa/font-awesome.min.css");
  @mixin center {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  header {
    margin-top: 0px;
    height: 150px;
    top: 0;
    position: sticky;
    width: 100vw;
    background-image: linear-gradient(
      to right,
      red,
      yellow,
      green,
      blue,
      purple,
      violet
    );
    z-index: 2;
  }
  nav {
    position: absolute;
    top: 0;
    width: 100vw !important;
    display: grid !important;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    color: white;
    background-color: #060606;
    align-items: center;
    height: calc(100% - 2px);
    h1 {
      margin-left: 10px;
      font-size: 37px;
      font-family: "Vibur", sans-serif;
      color: white;
      text-shadow: 0 0 13px #fff, 0 0 15px #fff, 0 0 26px #fff, 0 0 43px rgb(0, 247, 255),
        0 0 87px rgb(0, 255, 234), 0 0 92px #0fa, 0 0 102px #0fa, 0 0 151px #0fa;
    }
    .themechanger {
      display: flex;
      margin-left: auto;
      margin-right: 20px;
    }
    z-index: 2;
  }
</style>
