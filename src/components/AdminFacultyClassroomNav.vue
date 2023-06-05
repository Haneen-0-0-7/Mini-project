<template>
    <div class="navbar">
      <header>
        <div>
          <i class="fa-solid fa-users"></i>
        </div>
        <div class="logo">
          <h3>ALLOT FACULTY/CLASSROOM</h3>
        </div>
        <div class="btn-logout">
          <button @click="btnclick" id="logout">Back</button>
        </div>
      </header>
    </div>
  </template>
  
  <script>
  export default {
    name: "AdminAddFacultyNav",
    methods:{
      btnclick(){
        this.$router.push('/adminPage');
      }
    }
  };
  </script>
  
  <style scoped>
  
  *{
    padding:0;
    margin:0;
    box-sizing: border-box;
  }
    header{
      background-color: #000000;
      display:flex;
      padding:10px;
      margin-bottom: 10px;
      color:#ECECEC;
      justify-content: space-around;
      align-items: baseline;
      margin:0;
  }
  
  #logout{
          background-color: #FFD966;
          padding:8px 15px 8px 15px;
          color:black;
          border-radius:10%;
          display:flex;
          align-items: center;
          justify-content: space-around;
          font-family:Arial, Helvetica, sans-serif ;
          transition:0.5s;
          font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
  }
  #logout:hover{
          background-color: #000000;
          transform: translateY(-2px);
          border-color: aqua;
          color:white;
  }
  </style>