<script setup>
import { ref, onMounted } from 'vue';
import api from '../plugins/axios.js';
import SearchAddBar from '../components/SearchAddBar.vue'
import TaskCardList from '../components/TaskCardList.vue'

const tasks = ref([]);
const searchQuery = ref('');
const pendingTasks = ref([]);
const inProgressTasks = ref([]);
const completedTasks = ref([]);

onMounted(() => {
    getTasks();
}); 

const getTasks = () => {
    api.get('/tasks')
    .then((response) => {
        tasks.value = response.data;
        filterTasks(tasks)
    })
    .catch(error => {
        console.error("Erro ao buscar tarefas:", error);
    });
}

const handleSearch = (query) => {
  searchQuery.value = query;

  if (searchQuery.value == "") {
    getTasks()
    return;
  }

  api.get('/tasks/search/',
    {
        params: {
            title: searchQuery.value
        },
    }
  )
    .then((response) => {
        tasks.value = response.data;
        filterTasks(tasks)
    })
    .catch(error => {
        console.error("Erro ao buscar tarefas:", error);
    });
};

const filterTasks = (tasks) => {
    pendingTasks.value = tasks.value.filter(task => task.status === 'pending');
    inProgressTasks.value = tasks.value.filter(task => task.status === 'inProgress');
    completedTasks.value = tasks.value.filter(task => task.status === 'completed');
}
</script>
<template>
    <div class="taskList">
        <SearchAddBar @search="handleSearch"/>
        <div class="taskList__sections">
            <TaskCardList titleCard="Pendente" :tasks="pendingTasks"/>
            <TaskCardList titleCard="Em andamento" :tasks="inProgressTasks"/>
            <TaskCardList titleCard="Concluído" :tasks="completedTasks"/>
        </div>
    </div>
</template>
<style>
.taskList {
    width: 100%;
    height: 100vh;
    background-color: var(--backgound-color);
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.taskList__sections {
    width: 80%;
    height: 80vh;
    margin: 0rem auto;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
</style>