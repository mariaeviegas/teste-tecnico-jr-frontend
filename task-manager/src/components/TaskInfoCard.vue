<script setup>
import Swal from "sweetalert2";
import api from "../plugins/axios.js";

const items = [
    { title: 'Editar', action: 'edit' },
    { title: 'Excluir', action: 'delete' },
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

const deleteTask = (taskId) => {
    Swal.fire({
        title: "Tem certeza?",
        text: "Você não poderá reverter esta ação!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#d33",
        cancelButtonColor: "#3085d6",
        confirmButtonText: "Sim, excluir!",
        cancelButtonText: "Cancelar",
    }).then((result) => {
        if (result.isConfirmed) {
            api.delete(`/tasks/${taskId}`)
                .then(() => {
                    Swal.fire({
                        title: "Excluído!",
                        text: "A tarefa foi excluída com sucesso.",
                        icon: "success",
                        confirmButtonText: "OK",
                    }).then(() => {
                        location.reload();
                    });
                })
                .catch((error) => {
                    console.error(error);
                    Swal.fire({
                        title: "Erro!",
                        text: "Não foi possível excluir a tarefa. Tente novamente.",
                        icon: "error",
                        confirmButtonText: "OK",
                    });
                });
        }
    });
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
                        @click="item.action === 'delete' ? deleteTask(taskId) : editTask(taskId)"
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