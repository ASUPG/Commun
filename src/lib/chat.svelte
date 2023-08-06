<script lang="ts">
  import { initializeApp } from "firebase/app";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { config } from "./../fbaseconfig";
  import { getDownloadURL, getStorage, ref } from "firebase/storage";
  const storage = getStorage();
  const imgReference = ref(storage, "bestfriendsforever.jpg");
  getDownloadURL(imgReference).then((url) => {
    const img = document.getElementById('img1');
    img.setAttribute('src', url);
  })
  let app = initializeApp(config);
  let db = getFirestore(app);
  async function findGroups() {
    let groupRef = doc(db, "metadata", "groups");
    const groupSnap = await getDoc(groupRef);
    if (groupSnap.exists()) {
      let data = groupSnap.data();
      let newdata = [];
      for (const key in data) {
        newdata.push(data[key]);
      }
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
  <div id="sidenav" class="sidenav h-screen bg-slate-950">
    <div class="group bg-slate-800">
      <img src="" id="img1" class="groupicon" />
    </div>
  </div>
</chatwindows>

<style lang="scss">
  .group {
    width: 100%;
    height: 75px;
    display: flex;
    align-items: center;
  }
  .groupicon{
    width: 65px;
    height: 65px;
    border-radius: 50px;
  }
  .sidenav {
    position: absolute;
    padding-top: 150px;
    top: 0;
    z-index: 0;
    transition: 2s all;
    width: 30%;
    align-self: flex-start;
  }
  // reponsiveness
  @media only screen and (max-width: 900px) {
    .sidenav {
      left: 48px;
      width: 100% !important;
      transform: scaleX(0);
    }
  }
</style>
