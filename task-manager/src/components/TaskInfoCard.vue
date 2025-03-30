<script setup>
const items = [
    { title: 'Editar' },
    { title: 'Excluir' },
  ]
</script>
<template>
    <div class="taskInfoCard">
        <div class="taskInfoCard__mainInfo">
            <h1 class="mainInfo__title">ID - Titulo da tarefa</h1>
            <v-menu>
            <template v-slot:activator="{ props }">
              <v-btn icon="mdi-dots-horizontal" variant="text" v-bind="props"></v-btn>
            </template>

            <v-list>
              <v-list-item
                v-for="(item, i) in items"
                :key="i"
                :value="i"
              >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </div>

        <p class="taskInfoCard__description">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et
            dolore magna aliqua.
            Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
            Duis aute irure
            dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint
            occaecat cupidatat.
        </p>

        <div class="taskInfoCard__deadLine">
            <v-icon icon="mdi-calendar" />
            <p class="taskInfoCard__date">25/05/2025</p>
        </div>
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
    gap: 1rem;
}

.taskInfoCard__mainInfo {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
}

.mainInfo__title {
    font-size: 18px;
    margin: 0rem;
    color: var(--text-card-color);
}

.taskInfoCard__description {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
    font-weight: 200;
    line-height: 1.5;
}

.taskInfoCard__deadLine {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
}
</style>