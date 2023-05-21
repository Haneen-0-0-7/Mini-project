<template>
  <SetExamPageNav/>
  <SetExam/>
</template>

<script>
  import SetExamPageNav from "../components/AdminsetcancelexamNav.vue";
  import SetExam from "../components/Adminsetcancel.vue"
  export default{
      name:'adminsetcancel_',
      components:{
          SetExamPageNav,
          SetExam
      }
  }
</script>

<style scoped>

</style>