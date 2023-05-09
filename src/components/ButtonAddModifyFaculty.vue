<template>
    <button class="btn" @click="$emit('clicked')">{{ label }}</button>
  </template>
  
  <script>
  
  export default {
    props: {
      label: {
        type: String,
        default: "Button Label"
      }
    }
  };
  </script>
  
  <style scoped>
  .btn {
    padding: 10px 20px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  }
  </style>
  