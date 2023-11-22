<script lang="ts">
  // Importing Important Components
  export let params;
  import Header from "./lib/header.svelte";
  import ExtendButton from "./lib/extendbutton.svelte";
  import Annoucements from "./lib/ann.svelte";
  import { initializeApp } from "firebase/app";
  import {
    getFirestore,
    doc,
    getDoc,
    collection,
    onSnapshot,
    setDoc,
    getDocs,
  } from "firebase/firestore";
  import { config } from "./fbaseconfig";
  import { getDownloadURL, getStorage, ref } from "firebase/storage";

  // Declaring Important Variables
  let groups;
  let msg = [];
  let cookietosplit = `|${document.cookie}`;
  let splitedCookie = cookietosplit.split("|");
  let username = splitedCookie[2];
  let html = "";
  let isOpen = false;
  let oldIsOpen = isOpen;
  const storage = getStorage();
  let app = initializeApp(config);
  let db = getFirestore(app);
  let msgamount: number = 0;
  let toshow = false;
  let announcement = "";
  //Asking for PWA installation
  window.addEventListener("beforeinstallprompt", (e) => {
    e.preventDefault();
    let deferredPrompt = e;
    document.getElementById("askcont").style.display = "flex";
    document.getElementById("asktoinstall").addEventListener("click", (ce) => {
      deferredPrompt.prompt();
      deferredPrompt.userChoice.then((choice) => {
        if (choice.outcome === "accepted") {
          console.log("user accepted a2hs prompt");
        }
        deferredPrompt = null;
        document.getElementById("askcont").style.display = "none";
      });
    });
  });
  //Funtion to get the logos of the groups
  function getLogo(logo: string, count: number) {
    //Creating a refrence
    const pathRefrence = ref(storage, "logos/" + logo);
    getDownloadURL(pathRefrence).then((url) => {
      const img = document.getElementById(`logo${count}`);
      img.setAttribute("src", url);
    });
  }
  //Function to Retrieve All the Groups from the database
  async function findGroups() {
    let groupRef = doc(db, "metadata", "groups");
    const groupSnap = await getDoc(groupRef);
    if (groupSnap.exists()) {
      let data = groupSnap.data();
      let count_for_showing_images = 0;
      let newdata = [];
      for (const key in data) {
        newdata.push(data[key]);
        document.getElementById("sidenav").innerHTML += `
        <a class="group bg-slate-800" href="#/chat/${data[key].name}">
          <img src="" id="logo${count_for_showing_images}" class="groupicon" alt="" />
          <span class="groupname">${data[key].name}</span>
          </a>
          `;
        getLogo(data[key].logo, count_for_showing_images);
        count_for_showing_images++;
      }
      groups = newdata;
    } else {
      alert(
        "Critical Internal Server File Deleted(Internal Server Error)" + 500
      );
      throw (
        "Critical Internal Server File Deleted(Internal Server Error)" + 500
      );
    }
  }
  findGroups();
  //Saving announcements
  
  async function Savingannouncements() {
    let announeref = collection(db, "Announcements");
    let annSnap = await getDocs(announeref);
    annSnap.forEach((doc) => {
      announcement += doc.data().msg
    });
    console.log(announcement)
  }
  Savingannouncements()
  //Function to Disable Input bar
  async function disableWriting(grp: Array<any>) {
    let x = grp.length;
    let y = 0;
    while (y !== x) {
      console.log("Count", y);
      if (grp[y].name == params.group) {
        if (grp[y].availablefor !== "all") {
          if (grp[y].availablefor.includes(username)) {
            console.log("Includes");
            document.getElementById("msg").disabled = false;
          } else {
            console.log("Not Includes");
            document.getElementById("msg").disabled = true;
          }
        } else {
          document.getElementById("msg").disabled = true;
          console.log("Includes");
        }
        break;
      }
      y++;
    }
  }
  //Loading All the messages on arrival
  let msgRef = collection(db, params.group);
  disableWriting(groups);
  onSnapshot(msgRef, (snap) => {
    snap.docs.forEach((doc) => {
      if (username.split("name=")[1] != doc.data().sender) {
        html += `<div class="msg">
                <span class="sender">${doc.data().sender}<br></span>
                ${doc.data().msg}
                </div>`;
      } else {
        html += `<div class="msg sentbyme">
                  <span class="sender">${doc.data().sender}<br></span>
                  ${doc.data().msg}
                  </div>`;
      }
      msg.push({ ...doc.data(), id: doc.id });
    });
    document.getElementById("seamsg").innerHTML = html;
    html = "";
    msgamount = parseInt(msg[msg.length - 1].id);
    msg = [];
  });
  //Adding logic to open button and Change group
  if (params != undefined) {
    let oldGroup = params.group;
    setInterval(() => {
      if (params.group != oldGroup) {
        msg = [];
        console.log("yesy");
        disableWriting(groups);
        let colRef = collection(db, params.group);
        onSnapshot(colRef, (snap) => {
          snap.docs.forEach((doc) => {
            if (username.split("name=")[1] != doc.data().sender) {
              html += `<div class="msg">
                  <span class="sender">${doc.data().sender}<br></span>
                  ${doc.data().msg}
                  </div>`;
            } else {
              html += `<div class="msg sentbyme">
                  <span class="sender">${doc.data().sender}<br></span>
                  ${doc.data().msg}
                  </div>`;
            }
            msg.push({ ...doc.data(), id: doc.id });
          });
          document.getElementById("seamsg").innerHTML = html;
          html = "";
          msgamount = parseInt(msg[msg.length - 1].id);
          msg = [];
        });
      }
      oldGroup = params.group;
    }, 100);
  }
  setInterval(() => {
    if (isOpen != oldIsOpen) {
      if (isOpen) {
        document.getElementById("sidenav").style.transform = "scaleX(1)";
      } else {
        document.getElementById("sidenav").style.transform = "scaleX(0)";
        document.getElementById("areaformsg").style.width = "100%";
      }
      oldIsOpen = isOpen;
    }
  }, 100);
  //Fuction to send messages
  async function sendmsg() {
    const date = new Date();

    let day = date.getDate();
    let month = date.getMonth() + 1;
    let year = date.getFullYear();
    console.log("sent");
    let msgtosend = document.getElementById("msg").value;
    if (msgamount % 10 === 9) {
      console.log(`The value is${msgamount % 10}`);
      msgamount *= 10;
    } else {
      console.log(`The valuex is${msgamount + 1}`);
      msgamount += 1;
    }

    await setDoc(doc(db, params.group, `${msgamount}`), {
      msg: msgtosend,
      sender: username.split("name=")[1],
      onsent: `${day}-${month}-${year}`,
    });
  }
</script>

<div class="container-ask" id="askcont">
  <div class="border-ask">
    <div class="box-ask">
      <span class="text-ask">Please Install to Continue</span>
      <button type="button" class="ask" id="asktoinstall"
        ><span class="innerText"> Install </span></button
      >
    </div>
  </div>
</div>
<Annoucements content="Welcome" toshow="true" />
<Header />
<chatwindows style="display: flex;">
  <!-- Side Grouping Menu -->
  <ExtendButton bind:isOpen />
  <div id="sidenav" class="sidenav h-screen bg-slate-950" />
  <div class="areaformsg" id="areaformsg">
    <div class="seamsg" id="seamsg">
      <div class="msg"><span class="sender">ChatBot<br /></span>Welcome</div>
    </div>
    <div class="msgarea bg-slate-950">
      <input type="text" class="typemsg" id="msg" />
      <button class="send" type="button" on:click={sendmsg}
        ><svg
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          fill="#fff"
          height="25px"
          width="25px"
          version="1.1"
          id="Layer_1"
          viewBox="0 0 491.022 491.022"
          xml:space="preserve"
        >
          <g>
            <g>
              <path
                d="M490.916,13.991c-0.213-1.173-0.64-2.347-1.28-3.307c-0.107-0.213-0.213-0.533-0.32-0.747    c-0.107-0.213-0.32-0.32-0.533-0.533c-0.427-0.533-0.96-1.067-1.493-1.493c-0.427-0.32-0.853-0.64-1.28-0.96    c-0.213-0.107-0.32-0.32-0.533-0.427c-0.32-0.107-0.747-0.32-1.173-0.427c-0.533-0.213-1.067-0.427-1.6-0.533    c-0.64-0.107-1.28-0.213-1.92-0.213c-0.533,0-1.067,0-1.6,0c-0.747,0.107-1.493,0.32-2.133,0.533    c-0.32,0.107-0.747,0.107-1.067,0.213L6.436,209.085c-5.44,2.347-7.893,8.64-5.547,14.08c1.067,2.347,2.88,4.373,5.227,5.44    l175.36,82.453v163.947c0,5.867,4.8,10.667,10.667,10.667c3.733,0,7.147-1.92,9.067-5.12l74.133-120.533l114.56,60.373    c5.227,2.773,11.627,0.747,14.4-4.48c0.427-0.853,0.747-1.813,0.96-2.667l85.547-394.987c0-0.213,0-0.427,0-0.64    c0.107-0.64,0.107-1.173,0.213-1.707C491.022,15.271,491.022,14.631,490.916,13.991z M190.009,291.324L36.836,219.218    L433.209,48.124L190.009,291.324z M202.809,437.138V321.831l53.653,28.267L202.809,437.138z M387.449,394.898l-100.8-53.013    l-18.133-11.2l-0.747,1.28l-57.707-30.4L462.116,49.298L387.449,394.898z"
              />
            </g>
          </g>
        </svg></button
      >
    </div>
  </div>
</chatwindows>

<style lang="scss">
  .text-ask {
    color: white;
    font-family: "Vibur", sans-serif;
    font-size: x-large;
    margin-bottom: 20px;
    text-shadow: 0px 0px 7px #fff, 0px 0px 12px #fff, 0px 0px 15px #fff,
      0px 0px 25px rgb(146, 255, 241), 0px 0px 35px rgb(138, 251, 236);
  }
  .ask {
    height: 50px;
    width: 90px;
    border: white;
    border-radius: 20px;
    box-shadow: 0px 0px 7px #fff, 0px 0px 12px #fff, 0px 0px 15px #fff,
      0px 0px 25px rgb(146, 255, 241), 0px 0px 45px rgb(138, 251, 236),
      0px 0px 7px #fff inset, 0px 0px 12px #fff inset,
      0px 0px 15px rgb(138, 251, 236) inset;
    color: white;
    text-shadow: 0px 0px 10px #fff, 0px 0px 15px #fff;
    font-family: "Vibur", sans-serif;
  }
  #askcont {
    box-sizing: border-box;
    position: absolute;
    height: 100vh;
    width: calc(100vw + 20vw);
    z-index: 3;
    display: none;
    justify-content: center;
    align-items: center;
    left: 50%;
    transform: translateX(-50%);
  }
  .border-ask {
    background-image: linear-gradient(
      to bottom right,
      red,
      yellow,
      green,
      blue,
      purple,
      violet
    );
    height: 200px;
    width: 270px;
    border: 0px;
    border-radius: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box-ask {
    border-radius: 20px;
    height: 99%;
    width: 99%;
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .typemsg {
    width: 80%;
    height: 40px;
    border-radius: 20px;
    border: 1px solid white;
    background-color: rgba(255, 255, 255, 0.05);
    color: white;
    padding: 10px;
    font-size: 20px;
    box-shadow: 0px 0px 18px #fff, 0px 0px 22px #fff,
      0px 0px 32px rgb(0, 255, 221);
  }
  .send {
    height: 50px;
    width: 50px;
    margin-left: 10px;
  }
  // reponsiveness
  @media only screen and (max-width: 900px) {
    .sidenav {
      width: calc(100vw - 3rem) !important;
      transform: scaleX(0);
    }
    .areaformsg {
      width: calc(100vw - 3rem) !important;
      left: 3rem !important;
    }
  }
  @media only screen and (max-height: 950px) {
    .msgarea {
      height: 160px !important;
    }
    .send {
      margin-bottom: 60px;
    }
    .typemsg {
      margin-bottom: 60px;
    }
  }
</style>
