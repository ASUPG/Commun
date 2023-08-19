<script lang="ts">
  import { initializeApp } from "firebase/app";
  import { getFirestore, doc, getDoc } from "firebase/firestore";
  import { config } from "./../fbaseconfig";
  import { getDownloadURL, getStorage, ref } from "firebase/storage";
  const storage = getStorage();
  let app = initializeApp(config);
  let db = getFirestore(app);
  function showLogo(logo, count_for_showing_images) {
    console.log(logo, count_for_showing_images)
    const imgReference = ref(storage, `logos/${logo}`);
    getDownloadURL(imgReference).then((url) => {
      return url
    });
  }
  async function findGroups() {
    let groupRef = doc(db, "metadata", "groups");
    const groupSnap = await getDoc(groupRef);
    if (groupSnap.exists()) {
      let data = groupSnap.data();
      let count_for_showing_images = 0;
      let newdata = [];
      let logo = ""
      for (const key in data) {
        newdata.push(data[key]);
        logo = showLogo(data[key].logo, count_for_showing_images);
        document.getElementById("sidenav").innerHTML += `
          <div class="group bg-slate-800">
          <img src="${logo}" id="logo${count_for_showing_images}" class="groupicon" alt="" />
          <span class="groupname">${data[key].name}</span>
          </div>
        `;
        count_for_showing_images++;
      }
      return newdata;
    } else {
      alert(
        "Critical Internal Server File Deleted(Internal Server Error)" + 500
      );
      throw (
        "Critical Internal Server File Deleted(Internal Server Error)" + 500
      );
    }
  }
  let groups = findGroups();
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
  <div id="sidenav" class="sidenav h-screen bg-slate-950" />
</chatwindows>

<style lang="scss">
  // reponsiveness
  @media only screen and (max-width: 900px) {
    .sidenav {
      left: 48px;
      width: 100% !important;
      transform: scaleX(0);
    }
  }
</style>
