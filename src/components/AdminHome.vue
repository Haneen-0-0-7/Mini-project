<template>
  <div id="container-whole">
    <h1 id="title"></h1>
    <div id="container">
      <div id="button">
        <AddFacultyButton />
        <FacultyClassroom />
        <SetCancelExamButton/>
        <AttendenceButton />
        <MalpracticeRecordsButton/>
      </div>
    </div>
  </div>
</template>

<script>
import AddFacultyButton from './AddFacultyButton.vue';
import SetCancelExamButton from './SetCancelExamButton.vue';
import AttendenceButton from './AttendenceButton.vue';
import MalpracticeRecordsButton from './MalpracticeRecordsButton.vue';
import FacultyClassroom from './AdminFacultyClassroom.vue'
export default {
  name:'AdminHome',
  components:{
    AddFacultyButton,
    SetCancelExamButton,
    AttendenceButton,
    MalpracticeRecordsButton,
    FacultyClassroom
  }
};
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

#button {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.btn {
  margin: 10px;
  padding: 15px 25px;
  border-radius: 15%;
  border: 3px aquamarine solid;
  background-color: black;
  color: aquamarine;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana;
  transition:0.4s;
}

.btn:hover {
  background-color: aquamarine;
  color: black;
  border: 3px black solid;
  font-family: Georgia, 'Times New Roman', Times;
}
</style>
