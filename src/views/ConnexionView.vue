<template >
   <head> 
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,600,0,0"
    />
  </head>
  <body>
    <div class="login">
      <div class="avatar">
        <img src="https://xsgames.co/randomusers/avatar.php?g=male" />
      </div>
      <h2>Login</h2>
      <h3>Welcome back</h3>

      <form class="login-form">
        <div class="textbox">
          <input class="form-control" type="email" placeholder="Email*" name="email" v-model="email" />
          <span class="material-symbols-outlined"> account_circle </span>
        </div>
        <div class="textbox">
          <input class="form-control" type="password" placeholder="Password*" name="password" v-model="password" />
          <span class="material-symbols-outlined"> lock </span>
        </div>
        <span id = "error" style="color:red"> </span>
        <div><span id = "connexion" style="color:red;margin-left:7em"> </span></div>
        <button type="submit" @click="(e) => {connexion(e)}">LOGIN</button>
        <a href="https://website.com">Forgot your credentials?</a>
      </form>
    </div>
  </body>
</template>

<script>

export default {
    data (){
        return {
            email : "",
            password : "",
            users:[]
        }
    },
    methods:{
        connexion(e){
          e.preventDefault();
           if (
                 this.email == "" || this.password == ""
            ){
                document.querySelector('#error').innerHTML="« * champs obligatoires »";
            }
            else{
          fetch("http://localhost:3004/utilisateurs", {
            method: "GET", 
            headers : {"content-type": "application/json"} , 
          })
          .then(reponse => reponse.json())
          .then(data => {
            this.users = data;
            var check = false;
            data.map(user => {
              if(user.email == this.email && user.password == this.password){
                check = true;
                const commit = {
                  pseudo: user.pseudo,
                  email: user.email
                }
                console.log(commit);
                this.$store.commit({type: 'setUserConnected' , user : commit})
                if (localStorage.getItem("loggedIn") === "true")  {
                  this.$forceUpdate();
                  this.$emit('update_loggedIn')
                  this.$router.push('/')
                } 
              }
            })
            if(!check) document.querySelector('#connexion').innerHTML = "L'utilisateur n'existe pas, veuillez vérifier les informations saisies"
          })
        }   
        }
    }
}
</script>

<style>
* {
  box-sizing: border-box;
}
html,
body,
.wrapper {
  height: 100%;
}
@keyframes gradient {
  100% {
    background-size: 4000px 1000px;
  }
}
body {
  display: grid;
  place-items: center;
  margin: 0;
  padding: 0 24px;
  background-image: url("bg.svg");
  background-size: 2000px 1000px;
  background-position: -500px 0;
  color: #f9f9f9;
  font-family: "Euclid Circular A";
  animation: gradient 10s infinite alternate linear;
}
@media (width >= 500px) {
  body {
    padding: 0;
  }
}
.login {
  position: fixed;
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
@media (width >= 450px) {
  .login {
    width: 380px;
  }
}
.avatar {
  margin: 0 auto 16px;
  border-radius: 50%;
  background: linear-gradient(-45deg, #157ae1, #c7a1ff);
  padding: 2px;
  width: 120px;
  height: 120px;
}
.avatar > img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
  border: 4px solid #161616;
}
.login > h2 {
  font-size: 36px;
  font-weight: 500;
  margin: 0 0 4px;
}
.login > h3 {
  color: rgba(255, 255, 255, 0.38);
  margin: 0 0 30px;
  font-weight: 500;
  font-size: 16px;
}
.login-form {
  display: grid;
  gap: 16px;
  place-items: center;
  width: 100%;
  margin: 0;
}
.textbox {
  width: 100%;
  position: relative;
}
.textbox span {
  position: absolute;
  top: 50%;
  left: 16px;
  translate: 0 -50%;
  color: rgba(255, 255, 255, 0.38);
}
.login-form input,
.login-form button {
  width: 100%;
  height: 60px;
  outline: none;
  padding: 0;
  font-family: inherit;
  font-size: 16px;
  border-radius: 8px;
}
.login-form input {
  background: transparent;
  border: 2px solid rgba(255, 255, 255, 0.1);
  font-size: 18px;
  padding: 0 20px 0 50px;
  color: inherit;
}
.login-form input:focus {
  border-color: #157ae1;
}
.login-form input:focus ~ span {
  color: #157ae1;
}
.login-form button {
  cursor: pointer;
  background: #157ae1;
  color: #f9f9f9;
  border: 0;
  font-weight: 600;
  letter-spacing: 2px;
}
.login-form a {
  color: #157ae1;
  font-size: 16px;
  text-align: left;
  text-decoration: none;
}

</style>
