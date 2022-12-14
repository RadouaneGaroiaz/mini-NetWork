<template>
   <head>
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,600,0,0"
    />
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <!-- <img class="clouds" src="bg.svg" /> -->
    <div class="signup">
      <h2>Sign Up</h2>
      <h3>It's quick & simple</h3>
      <form class="form">
        <div class="textbox">
          <input type="text"  name="psoeudo" v-model="pseudo"  />
          <label>Pseudo*</label>
          <span class="material-symbols-outlined"> account_circle </span>
        </div>
        <div class="textbox">
          <input type="text" name="email" v-model="email"  />
          <label>Email*</label>
          <span class="material-symbols-outlined"> email </span>
        </div>
        <div class="textbox">
          <input type="password" name="password" id="password" v-model="password"  />
          <label>Password*</label>
          <span class="material-symbols-outlined"> key </span>
        </div>
        <div class="textbox">
          <input type="password" name="confirm_password" id="confirm_password" v-model="confirm_password"  />
          <label>Confirmer password*</label>
          <span class="material-symbols-outlined"> key </span>
        </div>
        <div class="textbox">
          <input type="text" name="urmImage" v-model="urlImgProfil"  />
          <label>Url de votre image - taille conseillée 1000x200px</label>
          <span class="material-symbols-outlined"> image </span>
        </div>
        <div class="inputs">
                    <span id = "error" style="color:red"> </span>
                </div>
                <div class="inputs">
                    <span id = "message" style="color:red"> </span>
                </div>
        <p>
          Signed up already?
          <a href="/connexion">Login here</a>
        </p>

        <button type="submit" @click="(e) => {inscrire(e)}">
          Créer un nouveau profil
          <span class="material-symbols-outlined"> arrow_forward </span>
        </button>
      </form>
    </div>
  </body>
</template>
<script>

export default {
    data (){
        return {
            pseudo : "",
            email : "",
            password : "",
            confirm_password : "",
            urlImgProfil : ""
        }
    },
    methods:{
       myAlert() {
  alert("L'utilisateur a été bien inscrit !");
},
        inscrire(e){
            e.preventDefault();
            if(this.password != this.confirm_password){
                document.querySelector('#message').innerHTML="Les mots de passe ne sont pas identiques !";
            }
            else if (
                this.pseudo == "" || this.email == "" || this.password == "" || this.confirm_password == ""
            ){
                document.querySelector('#error').innerHTML="« * champs obligatoires »";
            }
            else if (this.users.find(user => user.email == this.email)){
                document.querySelector('#error').innerHTML="L'email est déjà utilisé !";
            }
            else{
                const user = {
                    pseudo : this.pseudo,
                    email : this.email,
                    password : this.password,
                    urlImgProfil : this.urlImgProfil
                }
                fetch("http://localhost:3004/utilisateurs")
            .then(reponse => reponse.json())
            .then(data => {
                this.users = data ; 
        })
                fetch("http://localhost:3004/utilisateurs", {
                    method: "post", 
                    headers : {"content-type": "application/json"} , 
                    body : JSON.stringify(user)
                })
                .then(reponse => reponse.json())
                .then(data => {
                })
                this.pseudo =""
                this.email =""
                this.password =""
                this.confirm_password =""
                this.urlImgProfil =""
                this.myAlert();
                this.$router.push('/connexion');
            }
        }
    },
    computed:{
        
    },
    mounted:function(){
        fetch("http://localhost:3004/articles")
            .then(reponse => reponse.json())
            .then(data => {
                
                this.articles = data ; 
        })
        fetch("http://localhost:3004/utilisateurs")
            .then(reponse => reponse.json())
            .then(data => {
                this.users = data ; 
        })
    }
}
</script>
<style>
     * {
  box-sizing: border-box;
}
html,
body {
  height: 100%;
}
body {
  display: grid;
  place-items: center;
  margin: 0;
  padding: 0 20px;
  font-family: "Euclid Circular A";
}
button,
input {
  border: 0;
  width: 100%;
  height: 40px;
  background: transparent;
  font-family: inherit;
  font-size: 16px;
  outline: none;
}
@keyframes clouds {
  100% {
    translate: -50vw -55%;
    scale: 1 1.1;
  }
}
.clouds {
  position: fixed;
  top: 30%;
  left: 0;
  width: 3000px;
  height: 1500px;
  translate: 0% -50%;
  animation: clouds 15s infinite alternate linear;
}
.signup {
  position: fixed;
  height: 100%; 
  max-width: 460px;
  z-index: 3;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
  width: 90%;
  padding: 70px 30px 44px;
  border-radius: 22px;
  background: #161616;
  text-align: center;
}
.signup > h2 {
  font-size: 32px;
  font-weight: 600;
  margin: 0 0 6px;
  color: rgb(255 255 255 / 96%);
}
.signup > h3 {
  font-size: 16px;
  font-weight: 400;
  margin: 0 0 30px;
  color: rgb(255 255 255 / 40%);
}
.form {
  margin: 0;
  display: grid;
}
.textbox {
  position: relative;
  margin-bottom: 16px;
}
.textbox span {
  position: absolute;
  top: 50%;
  translate: 0 -50%;
  left: 0;
  font-size: 22px;
  pointer-events: none;
  color: rgb(255 255 255 / 40%);
}
.textbox input {
  padding: 0 24px 0 36px;
  border-bottom: 2px solid #2b3442;
  color: rgb(255 255 255 / 96%);
  height: 72px;
}
:is(input:focus, input:valid) ~ label {
  translate: -40px -40px;
  scale: 0.875;
}
input:focus ~ label {
  color: #216ce7;
}
input:focus {
  border-color: #216ce7;
}
:is(input:focus, input:valid) ~ span {
  color: rgb(255 255 255 / 96%);
}
.textbox label {
  position: absolute;
  top: 50%;
  left: 36px;
  translate: 0 -50%;
  color: rgb(255 255 255 / 40%);
  pointer-events: none;
  transition: 0.4s;
}
.form button {
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
  padding: 0 24px;
  border-radius: 6px;
  background: #216ce7;
  color: #f9f9f9;
  border: 0;
  font-family: inherit;
  font-weight: 600;
}
.signup p {
  margin: 0 0 22px;
  color: #778395;
}
.signup p > a {
  color: #216ce7;
  text-decoration: none;
}
</style>