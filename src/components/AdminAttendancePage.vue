<template>
  <div id="container-whole">
    <h1 id="title"></h1>
    <div id="container">
      <div class="dropdown-container">
        <div class="dropdown">
          <select
            v-model="selectedExam"
            class="styled-dropdown"
            style="width: 220px"
            @change="fetchClassNames"
          >
            <option value="" selected>Select the Exam</option>
            <option
              v-for="(exam, index) in exams"
              :key="index"
              :value="exam.examname"
            >
              {{ exam.examname }}
            </option>
          </select>
        </div>
      </div>

      <div class="dropdown-container" v-if="selectedExam">
        <div class="dropdown">
          <select
            v-model="classAllotted"
            class="styled-dropdown"
            style="width: 220px"
            @change="fetchClassDetails"
          >
            <option value="">Select a Class</option>
            <option
              v-for="className in classNames"
              :key="className"
              :value="className"
            >
              {{ className }}
            </option>
          </select>
        </div>
      </div>

      <h2 v-if="selectedExam" class="invigilator-name">Invigilator: {{ invigilatorName }}</h2>

      <h2 v-if="selectedExam" class="selected-heading">{{ classAllotted }}</h2>

      <table v-if="classAllotted" class="table">
        <thead>
          <tr>
            <th>Roll No</th>
            <th>Student Name</th>
            <th>Batch</th>
            <th>Present/Absent</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(student, index) in selectedExamDetails" :key="index">
            <td>{{ student.roll_no }}</td>
            <td>{{ student.student_name }}</td>
            <td>{{ student.batch }}</td>
            <td
              :style="{
                'background-color': student.attendance ? '#4caf50' : '#f44336',
                color: student.attendance ? 'black' : 'white',
              }"
            >
              {{ student.attendance ? "Present" : "Absent" }}
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
  name: "AdminAttendancePage",
  data() {
    return {
      selectedExam: "",
      exams: [],
      classAllotted: "", 
      selectedExamDetails: [], 
      classNames: [],
      invigilatorName: "",
    };
  },
  mounted() {
    this.fetchExams();
  },
  methods: {
    fetchExams() {
      axios
        .get("http://127.0.0.1:8000/setexam/get_exam_details/examname")
        .then((response) => {
          this.exams = response.data; 
        })
        .catch((error) => {
          console.error(error);
        });
    },
    fetchClassNames() {
      if (this.selectedExam) {
        axios
          .get(`http://127.0.0.1:8000/setexam/get_class/${this.selectedExam}/`)
          .then((response) => {
            this.classNames = response.data.class_names;
          })
          .catch((error) => {
            console.error(error);
          });
      } else {
        this.classNames = [];
      }
    },

    fetchClassDetails() {
      if (this.selectedExam) {
        axios
          .get(
            `http://127.0.0.1:8000/setexam/get_attendance/${this.selectedExam}/${this.classAllotted}/`
          )
          .then((response) => {
            this.selectedExamDetails = response.data.attendance_data;
            this.invigilatorName = response.data.faculty_name
            this.showTable = true;
          })
          .catch((error) => {
            console.error(error);
          });
      } else {
        this.selectedExamDetails = [];
        this.showTable = false;
      }
    },
  },
  watch: {
    selectedExam: {
      immediate: true, // Fetch class names when the component is mounted
      handler: "fetchClassNames",
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

.invigilator-name {
  margin-top: 1em;
  text-align: center;
  color: white;
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
</style>
