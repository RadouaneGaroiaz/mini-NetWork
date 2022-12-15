<script setup>
  import { RouterView } from 'vue-router'
</script>

<template>
 <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="./styles.css" />
  </head>
  <body>
    <aside class="sidebar">
      <div class="sidebar-inner">
        <header class="sidebar-header">
          <button
            type="button"
            class="sidebar-burger"
            @click="toggleSidebar()"
          ></button>
        </header>
        <nav class="sidebar-nav">
        
          <button type="button" @click="(e) => {homme(e)}">
          
            <img src="./assets/icon-home.svg" />
            <span style="animation-delay: 0.1s">Fils d'actualités</span>
          </button>
          <button v-if="loggedIn === false" type="button" @click="(e) => {creerProfil(e)}" >
           <img src="./assets/icon-accounts.svg" />
            <span style="animation-delay: 0.1s">Crée un profil</span>
          </button>
          <button v-if="loggedIn === false" type="button" @click="(e) => {seConnecter(e)}">
            <img src="./assets/icon-blockchain.svg" />
            <span style="animation-delay: 0.2s">Se connecter</span>
          </button>
        </nav>
        <footer class="sidebar-footer">
          <button type="button" @click="(e) => {deconnexion(e)}">
            <img src="./assets/icon-lock.svg" />
            <span>Logout</span>
          </button>
        </footer>
      </div>
    </aside>
    <div class="container">
        <RouterView @statusConnexion="reloadPage" @update_loggedIn="updateparent"/>
      </div> 

    
  </body>
</template>

<script>


  
  
  
export default {
  data () {
      return {
        loggedIn: false
      }
    },
    created() {
        if (localStorage.getItem("loggedIn") === "true")  {
          this.loggedIn = true
          
        }
    },
  methods:{

     toggleSidebar(){
      document.body.classList.toggle("open")
    },

    deconnexion(e){
      this.loggedIn = false
      e.preventDefault()
      this.$store.commit({type: 'setUserDeconnected'})
      this.$router.push({ path: '/connexion' })
      this.reloadPage()
    },

    creerProfil(e){
      this.loggedIn = false
      e.preventDefault()
      
      this.$router.push({ path: '/inscription' })
      this.reloadPage()
    },

     homme(e){
      this.$router.push({ path: '/' });
      this.reloadPage();
      //location.reload();
    }, 

    seConnecter(e){
      this.loggedIn = false
      e.preventDefault()
      
      this.$router.push({ path: '/connexion' })
      this.reloadPage()
    }, 


     reloadPage(){
      this.$forceUpdate();
      
    }, 
    updateparent() {
      if(localStorage.getItem("loggedIn") === "true")
        this.loggedIn = true
        else this.loggedIn = false
    }
  },
  beforeCreate: function(){
    var users = []
    fetch("http://localhost:3004/utilisateurs")
    .then(reponse => reponse.json())
    .then(data => {
        users = data ; 
    })
    fetch("http://localhost:3004/articles")
    .then(reponse => reponse.json())
    .then(data => {
      this.$store.articles = data ; 
      this.$store.articles.forEach(article => {
        users.forEach(user => {
          if(article.pseudo == user.pseudo) article.urlImgProfil = user.urlImgProfil
        });
      })
    })
  },
  mounted: function(){
    if (localStorage.getItem("loggedIn") === "true")  {
      this.loggedIn = true
    }else{
      this.loggedIn = false
      this.$forceUpdate()
    } 
  },
}
</script>

<style>
  input{
    margin-top:0.5em;
  }

  .sat{
    overflow: scroll;
    position: fixed;


  }


  * {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #121212;
}

button {
  background: transparent;
  border: 0;
  padding: 0;
  cursor: pointer;
}

.sidebar {
  position: fixed;
  overflow: hidden;
  top: 0;
  left: 0;
  width: 72px;
  height: 100%;
  background: #216ce7;
  transition: width 0.4s;
}

body.open .sidebar {
  width: 260px;
}

.sidebar-inner {
  position: absolute;
  top: 0;
  left: 0;
  width: 300px;
  height: inherit;
  display: flex;
  flex-direction: column;
  padding-bottom: 10px;
}

.sidebar-header {
  display: flex;
  align-items: center;
  height: 72px;
  padding: 0 1.25rem 0 0;
  background: rgba(0, 0, 0, 0.1);
}

.sidebar-burger {
  width: 72px;
  height: 72px;
  display: grid;
  place-items: center;
  background: url(./assets/icon-burger.svg) no-repeat center center;
  transition: 0.3s;
}

body.open .sidebar-burger {
  background: url(./assets/icon-close.svg) no-repeat center center;
}

.sidebar-logo {
  height: 20px;
  opacity: 0;
  transition: 0.3s;
}

body.open .sidebar-logo {
  opacity: 1;
}

.sidebar-nav {
  padding-top: 10px;
  flex: 1 1 auto;
}

.sidebar button {
  display: flex;
  gap: 25px;
  align-items: center;
  height: 50px;
  width: 72px;
  font-family: "Poppins";
  font-size: 16px;
  font-weight: 200;
  letter-spacing: 2px;
  line-height: 1;
  padding: 0 25px;
}

.sidebar button > img {
  width: 24px;
  height: 24px;
}

.sidebar button > span {
  color: #f9f9f9;
  opacity: 0;
  transition: 0.3s;
}

@keyframes appear {
  0% {
    opacity: 0;
    translate: 0 10px;
  }
  100% {
    opacity: 1;
    translate: 0;
  }
}

body.open .sidebar button > span {
  opacity: 1;
  animation: appear 0.3s both;
}

</style>
