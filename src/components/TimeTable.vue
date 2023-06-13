<template>
    <table>
      <thead>
        <tr>
          <th>Time</th>
          <th>Subject</th>
          <th>Location</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in timetableData" :key="index">
          <td>{{ item.time }}</td>
          <td>{{ item.subject }}</td>
          <td>{{ item.location }}</td>
        </tr>
      </tbody>
    </table>
  </template>
  
  <script>
  export default {
    data() {
      return {
        timetableData: [
          { time: '9:00 AM', subject: 'Mathematics', location: 'Room A' },
          { time: '10:30 AM', subject: 'Science', location: 'Room B' },
          { time: '1:00 PM', subject: 'English', location: 'Room C' },
          // Add more timetable data as needed
        ],
      };
    },
  };
  </script>
  