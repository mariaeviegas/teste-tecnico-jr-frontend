<script>
    import SearchAddBar from '../components/SearchAddBar.vue'
    import TaskCardList from '../components/TaskCardList.vue'

    export default {
        components:{
            SearchAddBar,
            TaskCardList,
        },
    };
</script>
<template>
    <div class="taskList">
        <SearchAddBar/>
        <div class="taskList__sections">
            <TaskCardList titleCard="Pendente"/>
            <TaskCardList titleCard="Em andamento"/>
            <TaskCardList titleCard="Concluído"/>
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