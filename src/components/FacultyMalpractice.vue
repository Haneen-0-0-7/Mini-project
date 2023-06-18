<template>
  <button @click="showModal = true" class="btn">Malpractice Register</button>
  <div v-if="showModal" class="modal">
    <div class="modal-content">
      <h2>Malpractice Form</h2>
      <form @submit.prevent="addFaculty">
        <div class="form-group">
          <label for="local-storage-value" class="form-label"
            >Invigilator</label
          >
          <input
            type="text"
            id="local-storage-value"
            :value="localStorageValue"
            readonly
            class="form-input"
          />
        </div>
        <div class="dropdown-container">
          <div class="dropdown">
            <select
              v-model="SelectedStudent"
              id="selected-student"
              class="styled-dropdown"
              style="width: 220px"
            >
              <option value="" selected>Choose a Roll No</option>
              <option v-for="(roll, index) in rolls" :key="index" :value="roll">
              {{ roll }}
            </option>
            </select>
          </div>
        </div>
        <div class="form-group">
          <label for="remarks" class="form-label">Remarks (10 words)</label>
          <input
            type="text"
            id="remarks"
            placeholder="Enter remarks"
            v-model="Remarks"
            class="form-input"
          />
        </div>
        <div class="button-container">
          <button type="submit" class="btnsubmit" @click="addmalpractice">
            Add
          </button>
          <button class="btnsubmit" @click="showModal = false">Close</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "FacultyMalpractice",
  data() {
    return {
      showModal: false,
      SelectedStudent: "",
      Invigilator: "",
      Remarks: "",
      sectionid: "",
      rolls:[],
    };
  },
  mounted() {
    this.Invigilator = localStorage.getItem("facultyname");
    this.sectionid = localStorage.getItem("sectionId");
    this.fetchRoll();
  },

  computed: {
    localStorageValue() {
      return localStorage.getItem("facultyname");
    },
  },
  methods: {
    refreshData() {
      axios.get("http://127.0.0.1:8000/faculty").then((response) => {
        this.faculties = response.data;
      });
    },
    fetchRoll() {
      axios
        .get(`http://127.0.0.1:8000/invigilator/malpractice/get_roll/${this.sectionid}`)
        .then((response) => {
          this.rolls = response.data.roll_numbers;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    addFaculty() {
      // Add logic to send the form data to the server
      const data = {
        StudentRegister: this.SelectedStudent,
        StudentInvigilator: this.Invigilator,
        StudentRemark: this.Remarks,
      };
      console.log(data);
      axios.post("http://127.0.0.1:8000/malpractice", data).then((response) => {
        alert(response.data);
        this.refreshData();
      });
      // Reset the form fields and close the modal
      this.SelectedStudent = "";
      this.Remarks = "";
      this.Invigilator = "";
      this.showModal = false;
    },
  },
};
</script>

<style scoped>
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

.button-container {
  display: flex;
  justify-content: space-around;
  margin-top: 16px;
}

input[type="text"],
input[type="password"],
input[type="email"] {
  display: block;
  margin-bottom: 10px;
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
  padding: 5em;
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
  display: flex;
  justify-content: center;
  margin-bottom: 1em;
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

.styled-dropdown {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 8px;
  font-size: 14px;
  width: 100%;
  height: auto;
  max-height: 160px;
  overflow-y: auto;
}

.dropdown-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 1em;
  margin-bottom: 1.5em;
}

.form-group {
  margin-bottom: 16px;
}

.form-label {
  display: block;
  font-weight: bold;
  margin-bottom: 4px;
}

.form-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
