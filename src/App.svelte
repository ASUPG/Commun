<script lang="ts">
  // Importing sweet alert
  // import {swal} from "sweetalert"
  // Including required dependencies
  import { initializeApp } from "firebase/app";
  // Importing firebase configuration
  import {config} from "./fbaseconfig.js"
  // import { writable } from "svelte/store";
  import { getAuth, signInWithPopup, GoogleAuthProvider, useDeviceLanguage } from "firebase/auth";
  // Initilizing the firebase app
  initializeApp(config);
  // Definning Needed Variable
  let isLoged: boolean;
  const provider = new GoogleAuthProvider();
  const auth = getAuth();
  let profilepic: string
  let ExpnadMenucount: number = 0;
  // Function for authentication
  function gauth() {
    signInWithPopup(auth, provider);
  }
  // Adding onAuthStateChange
  auth.onAuthStateChanged((auth) => {
    if (auth === null) {
      isLoged = false;
    } else {
      isLoged = true;
      profilepic = auth.photoURL
    }
  });
  // Function to expand user menu
  const expandUserMenu: () => void =  () => {
    ExpnadMenucount ++
    if (ExpnadMenucount %2 !== 0) {
      document.getElementById("usrmenu").style.height = "230px"    
      document.getElementsByClassName("usrmenu")[0].style.opacity = "1"
    } else {
      document.getElementById("usrmenu").style.height = "0px"
      document.getElementsByClassName("usrmenu")[0].style.opacity = "0"


    }
  }
</script>
<!-- Header -->
<header>
  <nav>
    <h1 class="brand-heading">Friends Forever</h1>
    <div class="account">
      <!-- Profile Picture -->
      {#if isLoged === false}
      <button class="login" type="submit" on:click={gauth}>
        <img src="google.svg" alt="Google" />
      </button>
      {:else}
      <button class="profpic" type="submit" on:click={expandUserMenu} >
        <img src="{profilepic}" alt="URL Pic" />
      </button>
      {/if}
    </div>
  </nav>
</header>
<!-- Main Body -->
<main>
  <div class="bg">
      <!-- User Menu -->
      <div id="usrmenu">
        <ul class="usrmenu flex flex-col items-center ma">
          <li>Yes</li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
    </div>
  </div>
</main>

<style lang="scss">
  #usrmenu{
    height: 0px;
    position: fixed;
    top: 148px;
    width: clamp(230px,230px,230px);
    border-radius: 0px 0px 0px 20px;
    left:calc(100vw - 230px);
    transition: 1s height;
    background-image: linear-gradient(45deg,red,yellow,green,blue,purple,violet);
  }
  .usrmenu{
    color: white;
    background-color: #060606;
    height: calc(100% - 2px);
    width: calc(100% - 2px);
    position: absolute;
    right: 0;
    border-radius: 0px 0px 0px 20px;
    opacity: 0;
    transition: 1s opacity;
    list-style: none;
  }
  header {
    margin-top: 0px;
    height: 150px;
    top: 0;
    position: fixed;
    width: 100vw;
    background-image: linear-gradient(to right,red,yellow,green,blue,purple,violet);
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
    }
    .account {
      display: flex;
      margin-left: auto;
      margin-right: 20px;
    }
  }
  .login, .profpic {
    background-color: transparent;
    border: transparent;
    transition: 0.5s transform;
    img{
      border-radius: 50px;
      aspect-ratio: 1/1;
      height: 70px;
    }
    &:hover{
      cursor: pointer;
    }
  }
  .login:hover{
    transform: rotate(45deg);

  }
</style>
