<script>
import TaskInfoCard from './TaskInfoCard.vue'
   export default {
    components : {
        TaskInfoCard
    },
    props: {
        titleCard: {
            type: String,
            required: true
        }
    }
   }
</script>
<template>
    <div class="taskCardList">
        <h1 class="taskCardList__title">{{ titleCard }}</h1>
        <TaskInfoCard/>
    </div>
</template>
<style>
    .taskCardList {
        width: 32%;
        padding: 1.5rem;
        background-color: white;
        border-radius: 1rem;
        display: flex;
        flex-direction: column;
        gap: 1.75rem;
    }

    .taskCardList__title {
        font-size: 20px;
        color: var(--primary-color);
    }
</style>