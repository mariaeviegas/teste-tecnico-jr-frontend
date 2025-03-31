<script setup>
const items = [
    { title: 'Editar' },
    { title: 'Excluir' },
];

defineProps({
    taskId: {
        type: Number,
        required: true,
    },
    taskTitle: {
        type: String,
        required: true,
    },
    taskDescription: {
        type: String,
        required: false,
    },
    taskStatus: {
        type: String,
        required: true,
    },
    taskDeadLine: {
        type: String,
        required: false,
    },
});

const formatDate = (dateString) => {
    if (!dateString) return null;
    const [year, month, day] = dateString.split('-');
    return `${day}/${month}/${year}`;
};
</script>
<template>
    <div :class="['taskInfoCard', taskStatus]">
        <div class="taskInfoCard__mainInfo">
            <h1 :class="['mainInfo__title', taskStatus]">{{ taskId }} - {{ taskTitle }}</h1>
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

        <p v-if="taskDescription != null" class="taskInfoCard__description">{{ taskDescription }}</p>

        <div v-if="taskDeadLine != null" class="taskInfoCard__deadLine">
            <v-icon icon="mdi-calendar" />
            <p class="taskInfoCard__date">{{ formatDate(taskDeadLine) }}</p>
        </div>
    </div>

</template>
<style scoped>
.taskInfoCard {
    height: max-content;
    padding: 1rem;
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.3s ease;
}

.taskInfoCard:hover {
    box-shadow: 0px 6px 10px rgba(0, 0, 0, 0.15);
}

.pending {
  background-color: var(--pending-status);
  color: var(--pending-status-title);
}

.inProgress {
  background-color: var(--inProgress-status);
  color: var(--inProgress-status-title);
}

.completed {
  background-color: var(--completed-status);
  color: var(--completed-status-title);
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
}

.taskInfoCard__description {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
    font-weight: 200;
    line-height: 1.5;
    margin: 0.5rem 0rem 0.75rem;
}

.taskInfoCard__deadLine {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
}
</style>