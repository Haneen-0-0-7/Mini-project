<template>
  <div id="container-whole">
    <h1 id="title">RECORDS</h1>
    <div id="container">
      <table class="table">
        <thead>
          <tr>
            <th>ExamName</th>
            <th>ExamDate</th>
            <th>ExamTime</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(exam, index) in exams" :key="index">
            <td>{{ exam.exam_name }}</td>
            <td>{{ exam.date }}</td>
            <td>{{ exam.time }}</td>
            <td>
              <button class="delete-button" @click="deleteRecord(exam.exam_id)">
                Delete Record
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      exams: [],
      selectedExam: "",
      selectedExamName: "",
      classAllotted: "",
      showTable: true,
    };
  },
  mounted() {
    this.getExamNames();
  },
  methods: {
    getExamNames() {
      axios
        .get("http://127.0.0.1:8000/setexam/exam-names")
        .then((response) => {
          this.exams = response.data.exam_details;
          console.log(this.exams);
        })
        .catch((error) => {
          console.error(error);
        });
    },
    deleteRecord(examId) {
      axios
        .delete(`http://127.0.0.1:8000/setexam/delete/${examId}/`)
        .then((response) => {
          response
          alert('The Exam Records Have been Deleted Successfully')
          this.getExamNames();
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
.button-container {
  display: flex;
  justify-content: center;
  margin-top: 2em;
}

.submit-button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #45a049;
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
  margin-top: 2em;
}

#container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.selected-heading {
  display: flex;
  margin-top: 2em;
  justify-content: center;
  color: white;
}

.table {
  margin-top: 2em;
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
  color: white;
}
.delete-button {
  background-color: #ff4c4c;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-button:hover {
  background-color: #ff0000;
}

</style>
