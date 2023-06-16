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
            >
              <option value="">Select an Exam</option>
              <option
                v-for="exam in visibleExams"
                :key="exam.exam_id"
                :value="exam.exam_id"
              >
                {{ exam.examname }}
              </option>
            </select>
          </div>
          <!-- <button class="styled-submit-button" @click="loadSelectedExamDetails">
            Submit
          </button> -->
        </div>
        <div v-if="selectedExam && showTable" class="selected-container">
          <h2 class="examtitle">{{ selectedExamName }}</h2>
          <!-- <table>
            <thead>
              <tr>
                <th style="width: 10%;">Exam ID</th>
                <th style="width: 20%;">Exam Name</th> -->
                <!-- <th style="width: 15%;">Exam Date</th>
                <th style="width: 15%;">Exam Time</th>
                <th style="width: 20%;">CSV Files</th>
                <th style="width: 20%;">Faculty Allotted</th> -->
              <!-- </tr>
            </thead>
            <tbody>
              <tr>
                <td>{{ selectedExamDetails.exam_id }}</td>
                <td>{{ selectedExamDetails.exam_name }}</td>
              </tr>
            </tbody>
          </table> -->

          <table v-if="selectedExamDetails && selectedExamDetails.alloted_faculty && selectedExamDetails.alloted_faculty.length>0">
            <thead>
              <tr>
                <th>Class ID</th>
                <th>Faculty</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="faculty in selectedExamDetails.alloted_faculty" :key="faculty.classid">
                <td><a href="#" @click="downloadSeatsData(faculty.classid)">{{ faculty.classid }}</a></td>
                <td>{{ faculty.faculty }}</td>
              </tr>
            </tbody>
          </table>
              
          <!-- <div class="form-container">
            <button @click="submitForm">Submit</button>
          </div> -->
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    data() {
      return {
        selectedExam: "", // Holds the selected exam ID
        selectedExamName: "", // Holds the selected exam name
        exams: [], // Array to store the list of exams
        classAllotted: "", // Holds the value of the class allotted input field
        selectedExamDetails: [], // Array to store the details of the selected exam
        showTable: false, // Controls table visibility
      };
    },
    mounted() {
      this.fetchExams();
    },
    computed: {
      visibleExams() {
        return this.exams.slice(-8); // Limit the visible exams to the first 8 entries
      },
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
      submitForm() {
        // Perform your submit logic here
        // Store the values in the ClassAllotted model
      },
      loadSelectedExamDetails() {
        axios
          .get(
            `http://127.0.0.1:8000/setexam/get_exam_details/${this.selectedExam}`
          )
          .then((response) => {
            // this.selectedExamDetails = response.data;
            this.selectedExamDetails = JSON.parse(JSON.stringify(response.data));
            console.log(this.selectedExamDetails);
            this.showTable = true; // Show the table after loading selected exam details
          })
          .catch((error) => {
            console.error(error);
          });
      },
      downloadSeatsData(classId) {
      const url = `/setexam/download_seats_csv/${classId}/`;
      const link = document.createElement('a');
      link.href = url;
      link.download = 'seats_data.csv';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }

    },
    watch: {
      selectedExam() {
        this.selectedExamName =
          this.exams.find((exam) => exam.exam_id === this.selectedExam)
            ?.examname || "";
        this.showTable = false; // Hide the table when a different exam is selected
        this.loadSelectedExamDetails();
      },
    },
  };
  </script>
  
  <style scoped>
  #container-whole {
    background-image: linear-gradient(rgba(15, 15, 5, 0.9), rgba(15, 15, 5, 0.9)),
      url("../images/logo.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    margin: 0;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
  }
  
  #container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .dropdown-container {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 2em;
  }
  
  .styled-dropdown {
    /* Add your custom styles to make the dropdown look styled */
    /* For example: */
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 8px;
    font-size: 14px;
    width: 100%; /* Make the dropdown width fill the container */
    height: auto;
    max-height: 160px; /* Limit the maximum height to 160px */
    overflow-y: auto;
  }
  
  .examtitle{
    color: white;
  }

  .selected-container {
    margin-top: 2em;
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .selected-container h2 {
    margin-bottom: 1em;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
    color: rgb(0, 0, 0);/* Set the table font color to white */
   }
   
  table th,
  table td {
    padding: 8px;
    border: 1px solid #ccc;
    background-color: #d6d8e3;
    width: 65%; /* Increase the width of each column */
  }

  

  
  .form-container {
    margin-top: 2em;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .styled-submit-button {
    /* Add your custom styles for the submit button */
    /* For example: */
    padding: 8px 16px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .styled-submit-button:hover {
    background-color: #45a049;
  }
  
  .form-container label {
    display: block;
    margin-bottom: 0.5em;
  }
  
  .form-container input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .form-container button {
    margin-top: 1em;
    padding: 8px 16px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .form-container button:hover {
    background-color: #45a049;
  }
  </style>
