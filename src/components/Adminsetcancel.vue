<template>
  <div id="container-whole">
    <div id="container">
      <div class="contents">
        <h2 style="text-decoration: underline">ENTER EXAM DETAILS</h2>
        <div class="text">
          <label>Exam Name:</label>
          <input type="text" v-model="examName" />
        </div>
        <div class="text">
          <label>Date of Exam:</label>
          <input type="date" v-model="examDate" />
        </div>
        <label class="radiohead">Time of Examination:</label>
        <div class="radiobtn">
          <div>
            <input
              type="radio"
              id="forenoon"
              value="forenoon"
              v-model="examTime"
            />
            <label for="forenoon" class="radiolabel">Forenoon</label>
          </div>
          <div>
            <input
              type="radio"
              id="afternoon"
              value="afternoon"
              v-model="examTime"
            />
            <label for="afternoon" class="radiolabel">Afternoon</label>
          </div>
        </div>
        <div>
          <button
            type="button"
            class="finalsubmit"
            @click.prevent="examidsubmit"
          >
            Submit
          </button>
          <label class="yearslabel">Years Taking Part in Exam:</label>
          <div class="years-checkboxes">
            <div v-for="year in years" :key="year" class="years-checkbox">
              <input
                type="checkbox"
                :id="year"
                :value="year"
                v-model="selectedYears"
                :disabled="modals[year].isChecked"
                @change="openModal(year)"
              />
              <label :for="year">{{ year }}</label>
            </div>
          </div>
        </div>

        <div v-for="year in years" :key="year">
          <div v-if="modals[year].visible" class="modal floating-modal">
            <div class="modalhead">
              <div class="modalhead-left">
                <h3>{{ year }}</h3>
              </div>
            </div>
            <div class="batch-list">
              <div
                v-for="batch in batchList[year]"
                :key="batch"
                class="batch-item"
              >
                <input
                  type="checkbox"
                  :id="batch"
                  :value="batch"
                  v-model="modals[year].checked[batch]"
                  @change="openModal(year)"
                />
                <label :for="batch">{{ batch }}</label>
                <input
                  type="file"
                  class="upload-csv"
                  :disabled="!modals[year].checked[batch]"
                  @change="uploadCSV(year, batch, $event.target.files[0])"
                />
              </div>
            </div>
            <div class="row">
              <button type="button" @click="closeModal(year)">Close</button>
              <button type="button" @click="submitForm(year)">Submit</button>
            </div>
            <div v-if="modals[year].showDialog" class="modal floating-modal">
              <h3>Are you sure you want to submit?</h3>
              <div class="row">
                <button type="button" @click="modals[year].showDialog = false">
                  No
                </button>
                <button type="button" @click="dialogSubmit(year)">Yes</button>
              </div>
            </div>
          </div>
        </div>
        <button type="button" class="finalsubmit" @click.prevent="finalSubmit">
          Submit
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "adminsetcancel_",
  data() {
    return {
      examName: "",
      examDate: "",
      examTime: "",
      examId: "",
      years: ["Year 1", "Year 2", "Year 3", "Year 4"],
      selectedYears: [],
      modals: {
        "Year 1": {
          visible: false,
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
          disabled: {},
          checked: {},
        },
        "Year 2": {
          visible: false,
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
          disabled: {},
          checked: {},
        },
        "Year 3": {
          visible: false,
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
          disabled: {},
          checked: {},
        },
        "Year 4": {
          visible: false,
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
          disabled: {},
          checked: {},
        },
      },
      batchList: {
        "Year 1": [
          "COMPUTER SICENCE AND ENGINEERING",
          "ELECTRONICS AND COMUNICATION",
          "ELECTRONICS AND ELECTRICAL",
          "MECHANICAL ENGINEERING",
          "CIVIL ENGINEERING",
          "ARCHITECTURE",
          "PRODUCTION ENGINEERING",
          "CHEMICAL ENGINEERING",
        ],
        "Year 2": [
          "COMPUTER SICENCE AND ENGINEERING",
          "ELECTRONICS AND COMUNICATION",
          "ELECTRONICS AND ELECTRICAL",
          "MECHANICAL ENGINEERING",
          "CIVIL ENGINEERING",
          "ARCHITECTURE",
          "PRODUCTION ENGINEERING",
          "CHEMICAL ENGINEERING",
        ],
        "Year 3": [
          "COMPUTER SICENCE AND ENGINEERING",
          "ELECTRONICS AND COMUNICATION",
          "ELECTRONICS AND ELECTRICAL",
          "MECHANICAL ENGINEERING",
          "CIVIL ENGINEERING",
          "ARCHITECTURE",
          "PRODUCTION ENGINEERING",
          "CHEMICAL ENGINEERING",
        ],
        "Year 4": [
          "COMPUTER SICENCE AND ENGINEERING",
          "ELECTRONICS AND COMUNICATION",
          "ELECTRONICS AND ELECTRICAL",
          "MECHANICAL ENGINEERING",
          "CIVIL ENGINEERING",
          "ARCHITECTURE",
          "PRODUCTION ENGINEERING",
          "CHEMICAL ENGINEERING",
        ],
      },
    };
  },
  watch: {
    selectedYears: {
      handler(newSelectedYears) {
        for (const year in this.modals) {
          if (!newSelectedYears.includes(year)) {
            this.modals[year].visible = false;
          }
        }
      },
      deep: true,
    },
  },
  methods: {
    openModal(year) {
      if (this.modals[year].isChecked) return;
      this.modals[year].visible = true;
    },
    closeModal(year) {
      this.modals[year].visible = false;
      this.modals[year].showDialog = false;
      this.modals[year].isChecked = false;
      for (const batch in this.modals[year].checked) {
        this.modals[year].checked[batch] = false;
      }
      this.selectedYears = this.selectedYears.filter((item) => item !== year);
    },
    addText(year) {
      const text = this.modals[year].inputText.trim();
      if (text !== "") {
        alert("Batch feature is disabled for " + year);
        this.modals[year].inputText = "";
      }
    },
    async submitForm(year) {
      this.modals[year].showDialog = true;
    },

    finalSubmit() {
  axios
    .post("http://127.0.0.1:8000/setexam/upload_csv/allotment", {
      examId: this.examId,
    })
    .then((response) => {
      alert(response.data); 
    })
    .catch((error) => {
      alert(error.data)
    });
},

    uploadCSV(year, batch, file) {
      this.modals[year].file = file;
      console.log(
        `Uploading CSV for Year: ${year}, Batch: ${batch}, File: ${file.name}}`
      );
    },

    async examidsubmit() {
      const examData = {
        examName: this.examName,
        examDate: this.examDate,
        examTime: this.examTime,
      };
      console.log("Exam Data:", examData);
      try {
        const response = await axios.post(
          "http://127.0.0.1:8000/setexam/get_exam_details",
          examData
        );
        this.examId = response.data.exam_id;
        alert('Exam id pushed to database!');
      } catch (error) {
        alert(error.data);
      }
    },

    async dialogSubmit(year) {
      this.modals[year].showDialog = false;
      this.modals[year].visible = false;
      this.modals[year].isChecked = true;
      this.selectedYears = this.selectedYears.filter((item) => item !== year);

      try {
        for (const batch in this.modals[year].checked) {
          if (this.modals[year].checked[batch]) {
            const formData = new FormData();
            formData.append("exam_id", this.examId);
            formData.append("year_name", year);
            formData.append("exam_time", this.examTime);
            formData.append("branch_time", batch);
            formData.append("csv_file", this.modals[year].file);

            try {
              const response = await axios.post(
                "http://127.0.0.1:8000/setexam/upload_csv",
                formData
              );
              console.log(response.data);
            } catch (error) {
              console.error(error);
            }
          }
        }
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.modaltextfield {
  padding: 0.5em;
}
.contents {
  margin: 1.5em;
}
.radiolabel {
  font-size: 1.2em;
}

.yearslabel {
  font-size: 1.2em;
}

.radiohead {
  font-size: 1.3em;
}
.radiobtn {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-evenly;
  margin: 0.5em;
}

.text {
  display: flex;
  flex-direction: column;
  flex-basis: auto;
  padding: 0.5em;
  border-radius: 10px;
  margin-top: 0.5em;
  font-size: 1.3em;
}

.text input[type="text"],
input[type="date"] {
  padding: 0.6em;
  border-radius: 0.5em;
}

.batch-list {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 1em;
}

.delete-button {
  padding: 8px 16px;
  margin-left: 8px;
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #d32f2f;
}

.delete-button:focus {
  outline: none;
}
.batch-item label {
  flex-grow: 1;
}

.upload-csv {
  margin-left: 1em;
}

.batch-item {
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}

.batch-item input[type="checkbox"] {
  margin-right: 0.5em;
}

#container-whole {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-image: linear-gradient(rgba(15, 15, 5, 0.9), rgba(15, 15, 5, 0.9)),
    url("../images/logo.png");
  background-position: center;
  position: relative;
  background-size: cover;
  overflow-x: auto;
}
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
}

input[type="radio"] {
  transform: scale(1.3);
}
input[type="date"],
input[type="radio"],
input[type="checkbox"] {
  margin-bottom: 10px;
}

.text-div {
  margin-bottom: 10px;
}

.dialog {
  background-color: #f4f4f4;
  padding: 20px;
  margin-bottom: 20px;
}

button {
  padding: 10px 15px;
  margin-right: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

#container {
  max-width: 100%;
  margin: 1em;
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f6f1e9;
  border-radius: 10%;
}

.floating-modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 9999;
  max-width: 90%;
  max-height: 90%;
  overflow-y: auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.yearslabel {
  font-size: 1.2em;
  margin-bottom: 0.5em;
}

.years-checkboxes {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: center;
  gap: 1em;
}

.years-checkbox {
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}

.years-checkbox input[type="checkbox"] {
  margin-right: 0.5em;
}

@media screen and (min-width: 600px) {
  .years-checkbox {
    width: 50%;
  }

  .modal {
    color: black;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .modalhead {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 1em;
    flex-direction: column;
  }

  .column {
    display: flex;
    flex-direction: column;
    gap: 1em;
  }

  .text-div {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
  }

  .finalsubmit {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 1em;
  }

  .dialog {
    text-align: center;
  }

  .row {
    display: flex;
    justify-content: flex-end;
    margin-top: 1em;
  }
  .batch-list .batch-item:not(:last-child) {
    margin-bottom: 0;
  }
}
</style>
