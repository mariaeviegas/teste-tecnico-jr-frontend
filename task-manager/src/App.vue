<template>
    <v-app>
      <TaskList />
    </v-app>
  </template>
  
  <script>
  import TaskList from './views/TaskList.vue';
  
  export default {
    components: {
      TaskList,
    },
  };
  </script>