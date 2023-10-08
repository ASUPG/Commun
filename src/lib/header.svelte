<script lang="ts">
  import { initializeApp } from "firebase/app";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { config } from "./../fbaseconfig";
  let app = initializeApp(config);
  let db = getFirestore(app);
  async function askpwd() {
    let cookietosplit = `|${document.cookie}`;
    let splitedCookie = cookietosplit.split("|");
    if (splitedCookie.length >= 2) {
      let islogedin = splitedCookie[1]
      if (islogedin !== "isLogined=verfor934") {
        let pass = prompt("Please Enter The Password");
        if (pass !== "") {
          let passRef = doc(db, "users", pass);
          const passSnap = await getDoc(passRef);
          if (passSnap.exists()) {
            let name = passSnap.data().name;
            let cookie = "isLogined=verfor934"
            document.cookie = cookie;
            document.cookie += `|name=${name}`
            document.cookie += `|id=${pass}`
          } else {
            askpwd();
          }
        } else {
          askpwd();
        }
      }
    } else {
      askpwd()
    }
  }
  askpwd();
</script>

<div id="wrapper">
  <!-- Header -->
  <header>
    <nav>
      <h1 class="brand-heading">Friends Forever</h1>
      <div class="themechanger"><i class="fa-regular fa-circle-half-stroke" style="color: #ffffff;"></i></div>
    </nav>
  </header>
</div>

<style lang="scss">
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
    }
    .themechanger {
      display: flex;
      margin-left: auto;
      margin-right: 20px;
    }
    z-index: 2;
  }

</style>
