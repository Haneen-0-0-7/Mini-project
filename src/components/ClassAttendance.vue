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
            @change="onExamChange"
          >
            <option value="" selected>Select the Exam</option>
            <option v-for="(exam, index) in exams" :key="index" :value="exam">
              {{ exam }}
            </option>
          </select>
        </div>
      </div>

      <h2 v-if="selectedExam" class="selected-heading">{{ selectedExam }}</h2>

      <table v-if="selectedExam" class="table">
        <thead>
          <tr>
            <th>Student Id</th>
            <th>Student Name</th>
            <th>Roll No</th>
            <th>Batch</th>
            <th>Present/Absent</th>
            <th>Attendance</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(student, index) in students" :key="index">
            <td>{{ student.student_id }}</td>
            <td>{{ student.student_name }}</td>
            <td>{{ student.roll_no }}</td>
            <td>{{ student.batch }}</td>
            <td>
              <input
                type="checkbox"
                v-model="student.attendance"
                :id="'attendance-' + student.student_id"
              />
              <label :for="'attendance-' + student.student_id"></label>
            </td>
            <td>{{ student.attendance }}</td>
          </tr>
        </tbody>
      </table>

      <div v-if="selectedExam" class="button-container">
        <button class="submit-button" @click="submitAttendance">Submit Attendance</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      selectedExam: "",
      exams: [],
      faculty: "",
      students: [],
    };
  },
  mounted() {
    this.faculty = localStorage.getItem("facultyname");
    this.fetchExams();
  },

  methods: {
    fetchExams() {
      axios
        .get(`http://127.0.0.1:8000/invigilator/get_examnames/${this.faculty}`)
        .then((response) => {
          this.exams = response.data.exam_names;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    onExamChange() {
      axios
        .get(
          `http://127.0.0.1:8000/invigilator/get_attendance/${this.selectedExam}/${this.faculty}`
        )
        .then((response) => {
          this.students = response.data.attendance_data.map((student) => {
            return {
              ...student,
              attendance: false,
            };
          });
          // Store the section_id in local storage
          const sectionId = response.data.section_id;
          localStorage.setItem('sectionId', sectionId);
        })
        .catch((error) => {
          console.error(error);
        });
    },
    submitAttendance() {
      const attendanceData = this.students.map((student) => {
        return {
          student_id: student.student_id,
          attendance: student.attendance,
        };
      });

      axios
        .post(
          `http://127.0.0.1:8000/invigilator/submit_attendance/${this.selectedExam}/`,
          { attendance_data: attendanceData }
        )
        .then((response) => {
          console.log(response.data);
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
  color:white;
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
  color:white;
}
</style>