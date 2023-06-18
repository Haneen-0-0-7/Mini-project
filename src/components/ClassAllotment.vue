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
    </div>
  </div>
</template>

<script>

export default{
    name:'ClassAllotmentPage',
    components:{

    },
}
</script>

<style scoped>
    #container-whole{
    background-image: linear-gradient(rgba(15,15,5,0.9),rgba(15,15,5,0.9)),url("../images/logo.png");
    background-position:center;
    position: relative;
    background-size: cover;
    margin:0;
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

</style>