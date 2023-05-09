<template>
  <form autocomplete="off">
    <div class="container">
      <div class="loginbox">
        <h2>FACULTY LOGIN</h2>
        <div class="loginpanel">
          <div class="inputfield">
            <i class="fa-regular fa-user"></i>
            <input required type="text" placeholder="Username" v-model="text" />
          </div>
          <div class="inputfield">
            <i class="fa-regular fa-keyboard"></i>
            <input
              required
              type="password"
              placeholder="Password"
              v-model="pass"
              autocomplete="new-password"
            />
          </div>
        </div>
        <div class="loginbutton">
          <button type="submit" id="btnlogin" @click.prevent="submit">Login</button>
        </div>
      </div>
    </div>
  </form>
</template>

<script>
import axios from "axios";
export default {
  name: "LoginAdmin",
  data() {
    return {
      text: "",
      pass: "",
      flag: 0,
    };
  },
  methods: {
    submit() {
      const data = {
        FacultyName: this.text,
        FacultyPass: this.pass,
      };
      axios.post("http://127.0.0.1:8000/faculty/login", data)
        .then((response) => {
          if (response.data.success) {
            // Login successful, redirect to faculty page
            this.$router.push('/facultyPage');
          } else {
            alert("Invalid credentials");
          }
        })
        .catch((error) => {
          // Handle error
          console.error(error);
          alert("An error occurred");
        });
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  box-sizing: border-box;
  font-family: sans-serif;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-image: linear-gradient(rgba(5, 5, 60, 0.7), rgba(5, 5, 60, 0.5)),
    url("../images/logo.png");
  background-position: center;
  position: relative;
  background-size: cover;
}

.loginbox {
  padding: 3em;
  margin: 2em;
  background-color: #f6f1e9;
  border-radius: 10%;
}

.container h2 {
  color: #181d31;
  text-align: center;
  position: relative;
  margin-bottom: 50px;
  text-decoration: none;
}

.inputfield {
  margin: 20px 0;
  display: flex;
  border-radius: 3px;
  align-items: center;
}

input {
  width: 100%;
  padding: 10px 8px;
  border-radius: 6px;
  background: transparent;
  outline: 0;
  border: 0;
  margin: 10px;
}

.loginbutton {
  align-items: center;
  justify-content: center;
  display: flex;
}

#btnlogin {
  width: 100%;
  text-align: center;
  font-size: 1.2em;
  padding: 8px 16px;
  border-radius: 8px;
  background-color: #181d31;
  color: white;
}
#btnlogin:hover {
  color: #f9d923;
  transform: translateY(-0.07em);
  box-shadow: 0 2px 1em -0.4em;
}

#link {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 15px;
}
</style>
