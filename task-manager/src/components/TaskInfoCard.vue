<script setup></script>
<template>
    <div class="taskInfoCard">
        <div class="taskInfoCard__mainInfo">
            <h1 class="mainInfo__title">ID - Titulo da tarefa</h1>
            <!-- TODO: menu do car -->
        </div>

        <p class="taskInfoCard__description">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et
            dolore magna aliqua.
            Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
            Duis aute irure
            dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint
            occaecat cupidatat.
        </p>

        <p class="taskInfoCard__deadLine">25/05/2025</p>
    </div>

</template>
<style scoped>

.taskInfoCard {
    height: max-content;
    padding: 1rem;
    background-color: var(--pending-status);
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.mainInfo__title {
    font-size: 18px;
}

.taskInfoCard__description {
    display: -webkit-box;
    -webkit-line-clamp: 3; 
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
    font-weight: 200;
    line-height: 1.5;
}
</style>