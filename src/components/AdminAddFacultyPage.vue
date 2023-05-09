<template>
  <div id="container-whole">
    <h1 id="title"></h1>
    <div id="container">
      <div id="form-container">
        <h3 id="span">ADD-DELETE-UPDATE DETAILS</h3>
        <div id="buttonpanel">
          <button id="Add" class="btn" @click="showModal = true">
            ADD FACULTY
          </button>
          <div v-if="showModal" class="modal">
            <div class="modal-content">
              <h2>Add Faculty</h2>
              <form @submit.prevent="addFaculty">
                <input
                  required
                  class="input"
                  type="text"
                  id="username"
                  v-model="username"
                  placeholder="Username"
                />
                <br />
                <input
                  required
                  type="password"
                  class="input"
                  id="password"
                  v-model="password"
                  placeholder="Password"
                  autocomplete="new-password"
                />
                <br />
                <input
                  class="input"
                  type="email"
                  id="email"
                  v-model="email"
                  placeholder="Email"
                />
                <br />
                <div id="buttondiv">
                  <button type="submit" class="btnsubmit" @click="addfaculty">
                    Add
                  </button>
                  <button class="btnsubmit" @click="showModal = false">
                    Close
                  </button>
                </div>
              </form>
            </div>
          </div>
          <button id="View" class="btn" @click="showViewModal = true">
            VIEW FACULTIES
          </button>
          <div v-if="showViewModal" class="modal">
            <div class="modal-content">
              <h2>View Faculties</h2>
              <table class="table">
                <thead>
                  <tr>
                    <th class="head">Username</th>
                    <th class="head">Password</th>
                    <th class="head">Email</th>
                    <th class="head">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="f in faculties" v-bind:key="f.FacultyId">
                    <td>{{ f.FacultyName }}</td>
                    <td class="facultypass">{{ f.FacultyPass }}</td>
                    <td class="facultyemail">{{ f.FacultyEmail }}</td>
                    <td>
                      <!-- <button @click="editFaculty()" class="edit">Edit</button> -->
                      <button
                        @click="deleteFaculty(f.FacultyId)"
                        class="delete"
                      >
                        Delete
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
              <button @click="showViewModal = false" class="viewclose">
                Close
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "AdminAddFacultyPage",
  components: {},
  data() {
    return {
      showModal: false,
      showViewModal: false,
      username: "",
      password: "",
      email: "",
      faculties: [],
    };
  },
  methods: {
    refreshData() {
      axios.get("http://127.0.0.1:8000/faculty").then((response) => {
        this.faculties = response.data;
      });
    },
    closemodal() {
      this.showModal = false;
    },
    openmodal() {
      this.showModal = true;
    },
    addFaculty() {
      // Add logic to send the form data to the server
      const data = {
        FacultyName: this.username,
        FacultyPass: this.password,
        FacultyEmail: this.email,
      };

      axios.post("http://127.0.0.1:8000/faculty", data).then((response) => {
        alert(response.data);
        this.refreshData();
      });

      const emailData = {
        to: this.email, // Replace with the email entered by the user
        subject: "Faculty Account Details",
        text: `Dear faculty, your username is ${this.username} and your password is ${this.password}. Please log in and change your password immediately.`,
      };

      
    axios.post("http://127.0.0.1:8000/faculty", emailData)
      .then(() => {
        console.log('Email sent successfully!');
      })

      // Reset the form fields and close the modal
      this.username = "";
      this.password = "";
      this.email = "";
      this.showModal = false;
    },
    deleteFaculty(id) {
      axios.delete(`http://127.0.0.1:8000/faculty/${id}`).then((response) => {
        this.refreshData();
        alert(response.data);
        // Update your Vue.js data here as needed
      });
    },
  },

  mounted: function () {
    this.refreshData();
  },
};
</script>

<style scoped>
input[type="text"],
input[type="password"],
input[type="email"] {
  display: block;
  margin-bottom: 10px;
}

table {
  border-collapse: collapse;
  width: 100%;
}

th,
td {
  padding: 10px;
  text-align: left;
  border: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
}

.email-cell {
  padding-left: 20px;
}

h2 {
  display: flex;
  justify-content: center;
  margin-bottom: 0.5em;
}

#container-whole {
  background-image: linear-gradient(rgba(15, 15, 5, 0.9), rgba(15, 15, 5, 0.9)),
    url("../images/logo.png");
  background-position: center;
  position: relative;
  background-size: cover;
  margin: 0;
}

#title {
  text-align: center;
  color: white;
}

#container {
  display: flex;
  min-height: 100vh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

#form-container {
  padding: 1em 2em 2em 2em;
  margin: 2em;
  background-color: #f6f1e9;
  border-radius: 10%;
}
#buttonpanel {
  display: flex;
  flex-direction: row;
  margin: 1em 1em 2em 1em;
  justify-content: space-around;
}

#span {
  text-decoration: underline;
  display: flex;
  justify-content: center;
}

.btn {
  margin: 10px;
  padding: 15px 25px;
  border-radius: 15%;
  border: 3px aquamarine solid;
  background-color: black;
  color: aquamarine;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana;
  transition: 0.4s;
}

.btn:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
  font-family: Georgia, "Times New Roman", Times;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  padding: 3em;
}

form {
  display: flex;
  margin-top: 1em;
  justify-content: space-around;
  flex-direction: column;
}

#buttondiv {
  display: flex;
  justify-content: space-between;
}

.modal-content > h2 {
  justify-content: center;
}

.input {
  padding: 0.5em;
  border-radius: 12%;
}

.btnsubmit {
  padding: 0.5em 2em 0.5em 2em;
  background-color: black;
  color: aquamarine;
  border: 3px aquamarine solid;
}

.btnsubmit:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
}

.viewclose {
  display: flex;
  justify-content: flex-start;
  margin-top: 1em;
  padding: 0.2em 1em 0.2em 1em;
  background-color: black;
  color: aquamarine;
  border: 3px aquamarine solid;
}

.viewclose:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
}

.edit {
  background-color: green;
  color: white;
  margin-left: 1em;
}

.delete {
  background-color: red;
  color: white;
  margin-left: 1em;
}
</style>
