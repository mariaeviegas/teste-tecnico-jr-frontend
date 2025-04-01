<script setup>
import TaskInfoCard from './TaskInfoCard.vue'

defineProps ({
    titleCard: {
        type: String,
        required: true
    },
    tasks: {
        type: Array,
        required: true
    }
})
</script>
<template>
    <div class="taskCardList">
        <h1 class="taskCardList__title">{{ titleCard }}</h1>
        
        <TaskInfoCard 
            v-for="task in tasks" :key="task.id"
            :taskId="task.id"
            :taskTitle="task.titulo"
            :taskDescription="task.descricao"
            :taskStatus="task.status"
            :taskDeadLine="task.deadLine"
            :taskCreatedAt="task.created_at"
        />
    </div>
</template>
<style scoped>
.taskCardList {
    width: 32%;
    padding: 1.5rem;
    background-color: white;
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1.75rem;
    max-height: 80vh; 
    overflow-y: auto; 
}

.taskCardList::-webkit-scrollbar {
    width: 0.5rem;
}

.taskCardList::-webkit-scrollbar-track {
    background: #f1f1f1; 
    border-radius: 0.5rem;
}

.taskCardList::-webkit-scrollbar-thumb {
    background: var(--secondary-color);
    border-radius: 0.5rem;
}

.taskCardList__title {
    font-size: 20px;
    color: var(--primary-color);
}
</style>