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

        <!-- Modals -->
        <div v-for="year in years" :key="year">
          <div v-if="modals[year].visible" class="modal floating-modal">
            <div class="modalhead">
              <div class="modalhead-left">
                <h3>{{ year }}</h3>
                <div class="column">
                  <input
                    type="text"
                    placeholder="Enter the Batch"
                    v-model="modals[year].inputText"
                    class="modaltextfield"
                    @keyup.enter="addText(year)"
                  />
                  <sr
                    >Press Enter to add the batches below and upload CSV
                    files</sr
                  >
                </div>
              </div>
            </div>
            <div
              v-for="text in modals[year].texts"
              :key="text"
              class="text-div"
            >
              {{ text }}
              <input type="file" />
            </div>
            <div class="row">
              <button type="button" @click="closeModal(year)">Close</button>
              <button type="button" @click="submitForm">Submit</button>
            </div>
            <div v-if="showDialog" class="modal floating-modal">
              <h3>Are you sure you want to submit?</h3>
              <div class="row">
                <button type="button" @click="showDialog = false">No</button>
                <button type="button" @click="Dialogsubmit(year)">Yes</button>
              </div>
            </div>
          </div>
        </div>
        <button type="button" class="finalsubmit" @click.prevent="finalsubmit">
          Submit
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "adminsetcancel_",
  data() {
    return {
      examName: "",
      examDate: "",
      examTime: "",
      years: ["Year 1", "Year 2", "Year 3", "Year 4"],
      selectedYears: [],
      modals: {
        "Year 1": {
          visible: false, // Initialize visible property
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
        },
        "Year 2": {
          visible: false, // Initialize visible property
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
        },
        "Year 3": {
          visible: false, // Initialize visible property
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
        },
        "Year 4": {
          visible: false, // Initialize visible property
          inputText: "",
          texts: [],
          showDialog: false,
          isChecked: false,
        },
      },
      showDialog: false,
    };
  },
  watch: {
    selectedYears: {
      handler(newSelectedYears) {
        // Close the modal when a checkbox is unchecked
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
      this.showDialog=false;
      this.modals[year].visible = true;
    },
    closeModal(year) {
      this.modals[year].visible = false;
      this.modals[year].showDialog = false;
      this.modals[year].isChecked = false; // Reset isChecked to false
      this.selectedYears = this.selectedYears.filter((item) => item !== year);
    },
    addText(year) {
    const text = this.modals[year].inputText.trim();
    if (text !== "") {
      if (!this.modals[year].texts.includes(text)) {
        this.modals[year].texts.push(text);
      } else {
        alert("Duplicate entry");
      }
      this.modals[year].inputText = "";
    }
  },
    uploadCSV(year) {
      // Handle CSV file upload for the selected year
      year;
    },
    submitForm() {
      this.showDialog = true;
    },
    finalsubmit() {},
    Dialogsubmit(year) {
      this.modals[year].showDialog = false;
      this.modals[year].visible = false;
      this.modals[year].isChecked = true;
      this.selectedYears = this.selectedYears.filter((item) => item !== year);
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

/* Adjust the width based on your preference */
@media screen and (min-width: 600px) {
  .years-checkbox {
    width: 50%;
  }

  .modal {
    color: black; /* Align items to the top */
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
    margin: 2em;
    display: flex;
  }
}
</style>
