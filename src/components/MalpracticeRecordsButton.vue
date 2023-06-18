<template>
  <button @click.prevent="showViewModal = true" class="btn">Malpractice Records</button>
  <div v-if="showViewModal" class="modal">
    <div class="modal-content">
      <h2>Record of Malpractice</h2>
      <table class="table">
        <thead>
          <tr>
            <th class="head">RegisterNo</th>
            <th class="head">Remark</th>
            <th class="head">Invigilator</th>
            <th class="head">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(f) in faculties" :key="f.StudentId">
            <td>{{ f.StudentRegister }}</td>
            <td class="facultyemail">{{ f.StudentRemark }}</td>
            <td class="facultyemail">{{ f.StudentInvigilator }}</td>
            <td>
              <!-- <button @click="editFaculty()" class="edit">Edit</button> -->
              <button @click="deleteFaculty(f.StudentId)" class="delete">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <button @click="showViewModal = false" class="viewclose">Close</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "MalpracticeRecordsButton",
  data() {
    return {
      showViewModal: false,
      faculties: [],
    };
  },
  mounted() {
    axios.get('http://127.0.0.1:8000/malpractice')
      .then(response => {
        this.faculties = response.data;
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    refreshData() {
      axios.get("http://127.0.0.1:8000/malpractice").then((response) => {
        this.faculties = response.data;
      });
    },
    deleteFaculty(id) {
      axios.delete(`http://127.0.0.1:8000/malpractice/${id}`).then((response) => {
        this.refreshData();
        alert(response.data);
        // Update your Vue.js data here as needed
      });
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

.viewclose {
  display: flex;
  justify-content: flex-start;
  margin-top: 1em;
  padding: 0.2em 1em 0.2em 1em;
  background-color: black;
  color: aquamarine;
  border: 3px aquamarine solid;
}

.btn:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
  font-family: Georgia, "Times New Roman", Times;
}

.viewclose:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
}

.edit{
    background-color: green;
    color:white;
    margin-left: 1em;
}

.delete{
    background-color: red;
    color:white;
    margin-left: 1em;
}

.edit:hover,.delete:hover{
    cursor:pointer;
}

.table {
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
</style>
