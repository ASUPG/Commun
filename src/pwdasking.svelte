<script>
    import Header from "./lib/header.svelte"
</script>
<main>
    <Header/>
    <div class="pwdbox">
        <h2>Kindly enter the Password<h2>
        <input type="text" class="pwd" id="pwd">
        
    </div>
</main>
<style lang="scss">
    main {
      background-color: #0d0d0d;
      height: 100%;
      width: 100%;
      display: flex;
      color:white;
      .pwdbox{
        height: 200px;
        width: 300px;
        border-radius: 20px;
        position: absolute;
        top:60%;
        left:50%;
        transform: translate(-50%, -50%);
        border: 1px solid rgb(0, 251, 255);
        color:white;
        display: flex;
        flex-direction: column;
        align-items: center; 
        box-shadow: 4px 4px 4px 4px  rgb(0, 251, 255);
        h2{
            font-size:20px
        }
        .pwd{
            border-radius: 25px;
            position: relative;
            margin-top:25%;
            color:white;
            border:1px solid rgb(0, 251, 255);
            background-color: rgb(38, 41, 41);
            &::placeholder{
                color:rgb(125, 125, 125)
            }
        }
      }
    }
</style>