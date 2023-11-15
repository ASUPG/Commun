<script lang="ts">
  // Importing neccasry components
  import Rd from "./redirect.svelte"
  import Router, {location, link} from 'svelte-spa-router'
  import Pwd from "./pwdasking.svelte"
  import Header from "./lib/header.svelte"
  import Chat from "./chat.svelte"
  import { initializeApp } from "firebase/app";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { config } from "./fbaseconfig";
  // Registering Service Worker
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js')
  }
  //Initializing Variables
  let app = initializeApp(config);
  let db = getFirestore(app);
  //Asking Password
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
            document.getElementsByName('body')[0].style.display = 'block';
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

<Router routes={{
  '/': Rd,
  '/chat/:group': Chat,
  "/askpwd": Pwd
}}/>


