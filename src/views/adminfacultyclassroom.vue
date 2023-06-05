<template>
    <AdminFacultyClassroomNav/>
    <AdminFacultyClassroomPage/>
</template>

<script>
    import AdminFacultyClassroomNav from '../components/AdminFacultyClassroomNav.vue';
    import AdminFacultyClassroomPage from '../components/AdminFacultyClassroomPage.vue';
    export default{
        name:'adminaddfaculty_',
        components:{
            AdminFacultyClassroomNav,
            AdminFacultyClassroomPage,
        }
    }
</script>

<style scoped>

</style>