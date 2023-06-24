<script lang="ts">
  // Including required dependencies
  import { initializeApp } from "firebase/app";
  // Importing firebase configuration
  import { config } from "./../fbaseconfig.js";
  // import { writable } from "svelte/store";
  import {
    getAuth,
    signInWithPopup,
    GoogleAuthProvider,
    signOut,
  } from "firebase/auth";
  // Initilizing the firebase app
  initializeApp(config);
  // Definning Needed Variable
  let isLoged: boolean;
  let isNewUser: boolean;
  const provider = new GoogleAuthProvider();
  const auth = getAuth();
  let profilepic: string;
  let ExpnadMenucount: number = 0;
  let name: string;
  let isOpen: boolean;
  let usrmenu: any = document.getElementsByClassName("usrmenu");
  // Function for authentication
  function gauth() {
    signInWithPopup(auth, provider);
  }
  // Adding onAuthStateChange
  auth.onAuthStateChanged(async (user) => {
    if (user === null) {
      isLoged = false;
    } else {
      isLoged = true;
      profilepic = user.photoURL;
      name = user.displayName;
      isNewUser = user.metadata.creationTime === user.metadata.lastSignInTime;
    }
  });
  // Function to expand user menu
  const expandUserMenu: () => void = () => {
    ExpnadMenucount++;
    if (ExpnadMenucount % 2 !== 0) {
      document.getElementById("usrmenu").style.height = "230px";
      usrmenu[0].style.opacity = "1";
    } else {
      document.getElementById("usrmenu").style.height = "0px";
      usrmenu[0].style.opacity = "0";
    }
  };
  // Logout function
  let logout: () => void = async () => {
    let swal = await import("sweetalert")
    swal.default({
      title: "Do you really want to logout?",
      text: "Are you sure that you want to logout?",
      icon: "warning",
      dangerMode: true,
    }).then((willLogout) => {
      if (willLogout) {
        signOut(auth).then(() => {
          expandUserMenu();
          swal.default(
            "Loged Out",
            "You are now successfuly loged out. Log in to use the app",
            "success"
          );
        });
      }
    });
  };
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
        <button class="profpic" type="submit" on:click={expandUserMenu}>
          <img src={profilepic} alt="URL Pic" />
        </button>
      {/if}
    </div>
  </nav>
</header>
<!-- User Menu -->
<menu id="usrmenu">
  <div class="usrmenu flex flex-col items-center">
    <span class="font-bold text-lg flex justify-center items-center text-center"
      >Hello {name}</span
    >
    <span>
      <button
        on:click={gauth}
        class="logout h-5 w-40
            text-white rounded-2xl cursor-pointer"
        type="button">Switch User</button
      >
    </span>
    <span>
      <button
        on:click={logout}
        class="logout bg-red-600 h-10 w-20 font-bold
          text-white rounded-2xl"
        type="button">Log Out</button
      >
    </span>
  </div>
</menu>
<!-- Login Screen -->
{#if isNewUser === true}
<div class="loginscreen">
  <div class="enter-tag">
    <input type="text" name="" id="tag" placeholder="Enter your Tag Name">
    <input type="button" value="Done!">
  </div>
</div>
{/if}
<style lang="scss">
  @mixin center{
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .loginscreen{
    width: 100vw;
    height: 100vh;
    position: absolute;
    background-color: #00000090;
    z-index: 100;
    overflow: hidden;
    @include center;
    .enter-tag{
      height: 220px;
      background-color: #000000;
      @include center;
      width: 400px;
      border-radius: 50px;
      flex-direction: column;
      input[type=text]{
        background-color: #1c1c1c;
        color:white;
        padding-left: 20px;
        height: 50px;
        width:300px;
        border-radius: 100px;
        &::placeholder{
          color:white;
        }
        margin-bottom: 20px;
      }
      input[type=button]{
        background-color: #1c1c1c;
        color:white;
        width: 100px;
        height: 50px;
        border-radius: 50px;
      }
    }
  }
  #usrmenu {
    height: 0px;
    position: fixed;
    top: 148px;
    width: clamp(230px, 230px, 230px);
    border-radius: 0px 0px 0px 20px;
    left: calc(100vw - 230px);
    transition: 1s height;
    background-image: linear-gradient(
      45deg,
      red,
      yellow,
      green,
      blue,
      purple,
      violet
    );
    span {
      height: calc(100% / 3);
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
  .usrmenu {
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
    }
    .account {
      display: flex;
      margin-left: auto;
      margin-right: 20px;
    }
    z-index: 2;
  }
  .login,
  .profpic {
    background-color: transparent;
    border: transparent;
    transition: 0.5s transform;
    img {
      border-radius: 50px;
      aspect-ratio: 1/1;
      height: 70px;
    }
    &:hover {
      cursor: pointer;
    }
  }
  .login:hover {
    transform: rotate(45deg);
  }
</style>
