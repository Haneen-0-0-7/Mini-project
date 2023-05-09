<template>
  <form @submit.prevent = "submit" autocomplete="off">
    <div class="container">
      <div class="loginbox">
      <h2>ADMIN LOGIN</h2>
      <div class="loginpanel">
        <div class="inputfield">
          <i class="fa-regular fa-user"></i>
          <input required type="text" placeholder="Username" v-model="text"/>
        </div>
        <div class="inputfield">
          <i class="fa-regular fa-keyboard"></i>
          <input required type="password" placeholder="Password" v-model="pass" autocomplete="new-password"/>
        </div>
      </div>
      <div class="loginbutton">
        <button type="submit" id="btnlogin">Login</button>
      </div>
      <div id="link">
        <router-link to="/facultylogin">faculty?login here!</router-link>
      </div>
    </div>
  </div>
</form>
  </template>
  
  <script>
  export default {
    name: 'LoginAdmin',
    data(){
      return{
        text:'',
        pass:'',
      };
    },
    methods:{
      submit(){
        if(this.text==='admin' && this.pass==='admin') 
        {
          this.$router.push("/adminPage");
        }
        else{
          alert(`Incorrect Credentials`);
        }
      }
    }
  }
  </script>
  
  <style scoped>
  *{
    margin:0;
    padding:0;
    box-sizing: border-box;
   }

html{
    box-sizing: border-box;
    font-family: sans-serif;
}

.container{
    display:flex;
    flex-direction: column;
    align-items:center;
    justify-content: center;
    height:100vh;
    background-image: linear-gradient(rgba(5,5,60,0.7),rgba(5,5,60,0.5)),url("../images/logo.png");
    background-position:center;
    position: relative;
    background-size: cover;
}

.loginbox{
  padding:3em;
  margin:2em;
  background-color:#F6F1E9;
  border-radius: 10%;
}

.container h2{
    color:#181D31;
    text-align: center;
    position: relative;
    margin-bottom: 50px;
    text-decoration:none;
}

.inputfield{
    margin:20px 0;
    display:flex;
    border-radius: 3px;
    align-items: center;
}

input{
    width:100%;
    padding: 10px 8px;
    border-radius: 6px;
    background: transparent;
    outline: 0;
    border:0;
    margin:10px;
}

.loginbutton{
    align-items: center;
    justify-content: center;
    display: flex;
}

#btnlogin{
    width: 100%;
    text-align: center;
    font-size: 1.2em;
    padding:8px 16px;
    border-radius: 8px;
    background-color:#181D31;
    color: white;
}
#btnlogin:hover{
    color: #F9D923;
    transform: translateY(-0.07em);
    box-shadow: 0 2px 1em -0.4em ;
}

#link{
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top:15px;
}

</style>