<template>
    <button @click = "onClickAdd" class="btn">Class Attendance</button>
</template>

<script>

    export default{
        name:'FacultyAttendance',
        methods:{
          onClickAdd(){
            this.$router.push('/facultyclassattendance')
          }
        }
    }
</script>

<style scoped>

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