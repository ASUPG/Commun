<script lang="ts">
  if (document.cookie !== "isLogined=verfor934") {
    let pass = prompt("Please Enter The Password");
    if (pass === "fforever") {
      document.cookie = "isLogined=verfor934";
    } else {
      document.getElementsByTagName("body")[0].style.display = "none";
    }
  }
</script>

<div id="wrapper">
  <!-- Header -->
  <header>
    <nav>
      <h1 class="brand-heading">Friends Forever</h1>
      <div class="account" />
    </nav>
  </header>
</div>

<style lang="scss">
  @mixin center {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .loginscreen {
    width: 100vw;
    height: 100vh;
    position: absolute;
    background-color: #00000090;
    z-index: 100;
    overflow: hidden;
    @include center;
    .enter-tag {
      height: 220px;
      background-color: #000000;
      @include center;
      width: 400px;
      border-radius: 50px;
      flex-direction: column;
      input[type="text"] {
        background-color: #1c1c1c;
        color: white;
        padding-left: 20px;
        height: 50px;
        width: 300px;
        border-radius: 100px;
        &::placeholder {
          color: white;
        }
        margin-bottom: 20px;
      }
      input[type="button"] {
        background-color: #1c1c1c;
        color: white;
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
