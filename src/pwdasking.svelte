<script>
  import Header from "./lib/header.svelte";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { initializeApp } from "firebase/app";
  import { config } from "./fbaseconfig";
  let firebase = initializeApp(config)
  let db = getFirestore(firebase)
  async function authpwd(){
    console.log("yes")
    let pass = document.getElementById("pwd").value
    if (pass !== "") {
          let passRef = doc(db, "users", pass);
          const passSnap = await getDoc(passRef);
          if (passSnap.exists()) {
            let name = passSnap.data().name;
            let batch = passSnap.data().batch;
            let cookie = "isLogined=verfor934"
            document.cookie = cookie;
            document.cookie += `|name=${name}`
            document.cookie += `|id=${pass}`
            document.cookie += `|${batch}`
            window.location.href = "/#/"
    }
  }
}
</script>

<Header />
<main>
  <div class="pwdbox">
    <h2>
      Kindly enter the<br /> <span class="pwdtext">Password</span>
    </h2>
    <input type="text" class="pwd" id="pwd" placeholder="Enter Password" />
    <input type="submit" value="Submit" class="pwd subbtn" id="subbtn" on:click={authpwd} />
  </div>
</main>

<style lang="scss">
  @import url("https://fonts.googleapis.com/css2?family=Agbalumo&family=Vibur&display=swap");
  main {
    background-color: #0d0d0d;
    height: calc(100% - 150px);
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    .pwdbox {
      height: 350px;
      width: 300px;
      border-radius: 20px;
      border: 1px solid #00fbff;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center !important;
      box-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px #fff, 0 0 42px #00fbff,
        0 0 82px #00fbff, 0 0 92px #00fbff, 0 0 102px #0fa, 0 0 151px #0fa;
      .pwdtext {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
      }
      h2 {
        position: absolute;
        left: 50%;
        font-size: 28px;
        transform: translateX(-50%);
        margin-top: 20px;
        font-family: "Vibur";
        text-shadow: 0px 0px 14px #fff, 0px 0px 23px #fff, 0px 0px 41px #fff,
          0px 0px 82px #00fbff, 0px 0px 112px #00fbff;
      }
      .pwd {
        border-radius: 25px;
        width: 250px;
        height: 40px;
        color: white;
        border: 1px solid rgb(0, 251, 255);
        background-color: rgb(38, 41, 41);
        position: absolute;
        top: 60%;
        left: 50%;
        transform: translate(-50%, -50%);
        box-shadow: 0px 0px 18px #fff, 0px 0px 28px rgb(0, 251, 255),
          0px 0px 38px rgb(0, 253, 207);
        &::placeholder {
          color: rgb(125, 125, 125);
        }
        padding:7px;
      }
      .subbtn{
        width:100px !important;
        margin-top:100px;
        cursor:pointer;
      }
    }
  }
</style>
