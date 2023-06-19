<template>
    <AdminAttendanceNav/>
    <AdminAttendancePage/>
</template>

<script>
    import AdminAttendanceNav from '../components/AdminAttendanceNav.vue';
    import AdminAttendancePage from '../components/AdminAttendancePage.vue';
    export default{
        name:'adminaddfaculty_',
        components:{
            AdminAttendanceNav,
            AdminAttendancePage,
        }
    }
</script>

<style scoped>

</style>