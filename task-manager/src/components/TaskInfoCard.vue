<script setup>
import Swal from "sweetalert2";
import api from "../plugins/axios.js";
import { ref } from "vue";

const dialog = ref(false);

const items = [
    { title: 'Editar', action: 'edit' },
    { title: 'Excluir', action: 'delete' },
];

const menu = ref(false);
const id = ref()
const title = ref('');
const description = ref('');
const status = ref('');
const selectedDate = ref('');
const createdAt = ref('');
const errorMessage = ref('');

const statusOptions = [
    { text: 'Pendente', value: 'pending' },
    { text: 'Em andamento', value: 'inProgress' },
    { text: 'Concluído', value: 'completed' },
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
    taskprazo: {
        type: String,
        required: false,
    },
    taskCreatedAt: {
        type: String,
        required: true
    }
});

const formatDateForDisplay = (dateString) => {
    if (!dateString) return null;
    const [year, month, day] = dateString.split('-');
    return `${day}/${month}/${year}`;
};

function formatToDate(date) {
    const data = new Date(date);
    return data.toISOString().split('T')[0];
  }

const openEditTask = (taskId, taskTitle, taskDescription, taskStatus, taskprazo, taskCreatedAt) => {
    id.value = taskId;
    title.value = taskTitle;
    description.value = taskDescription;
    status.value = taskStatus;
    selectedDate.value = taskprazo;
    createdAt.value = taskCreatedAt
    dialog.value = true;
}

const closeModal = () => {
    title.value = '';
    description.value = '';
    status.value = '';
    selectedDate.value = '';
    errorMessage.value = '';
    dialog.value = false;
  };

const updateTask = () => {
    if (!title.value || !status.value) {
      errorMessage.value = '*Por favor, preencha todos os campos obrigatórios.';
      return;
    }

    const data = {
      titulo: title.value,
      descricao: description.value,
      status: status.value.value,
      prazo: selectedDate.value? formatToDate(selectedDate.value) : null,
    };

    api.put(`/tasks/${id.value}`, data)
    .then(() => {
      closeModal();

      Swal.fire({
        title: "Tarefa atualizada",
        text: "Os dados da tarefa foram atualizados com sucesso!",
        icon: "success",
        confirmButtonText: "OK",
      }).then(() => {
        location.reload();
      });
    })
    .catch((error) => {
      closeModal();

      Swal.fire({
        title: "Ocorreu algum erro",
        text: "Os dados da tarefa não foram atualizados devido algum erro. Tente novamente!",
        icon: "error",
        confirmButtonText: "OK",
      }).then(() => {
        location.reload();
      });
    })
}

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
                        @click="item.action === 'delete' ? deleteTask(taskId) : openEditTask(taskId, taskTitle, taskDescription, taskStatus, taskprazo, taskCreatedAt)"
                    >
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item>
                </v-list>
            </v-menu>
        </div>

        <p v-if="taskDescription != null" class="taskInfoCard__description">{{ taskDescription }}</p>

        <div v-if="taskprazo != null" class="taskInfoCard__prazo">
            <v-icon icon="mdi-calendar" />
            <p class="taskInfoCard__date">{{ formatDateForDisplay(taskprazo) }}</p>
        </div>
    </div>


    <v-dialog v-model="dialog" max-width="1000">
    <v-card title="Editar tarefa" style="padding: 1.5rem;">
      <v-card-text>
        <v-column style="gap: 1.5rem;">
          <v-text-field v-model="title" clearable label="*Título" variant="outlined" required></v-text-field>
          <v-textarea v-model="description" clearable label="Descrição" variant="outlined"></v-textarea>

          <v-row>
            <v-col cols="6">
            <v-text-field v-model="createdAt" label="Data de criação" variant="outlined" disabled></v-text-field>
            </v-col>
            <v-col cols="6">
              <v-menu v-model="menu" transition="scale-transition" min-width="auto">
                <template v-slot:activator="{ props }">
                  <v-text-field
                    v-bind="props"
                    label="Prazo"
                    prepend-inner-icon="mdi-calendar"
                    readonly
                    clearable
                    v-model="selectedDate"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="selectedDate" @update:model-value="menu = false"></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
          <v-select
                v-model="status"
                :items="statusOptions"
                item-title="text"
                item-value="value"
                label="*Status"
                chips
                return-object="false"
              ></v-select>
        </v-column>

        <small v-if="errorMessage" style="color: red;">{{ errorMessage }}</small>
        <small v-else class="text-caption text-medium-emphasis">*indica um campo obrigatório</small>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text="Fechar" variant="plain" @click="closeModal"></v-btn>
        <v-btn 
            style="background-color: var(--secondary-color); color: white;" 
            text="Editar" 
            variant="tonal" 
            @click="updateTask()"
        ></v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
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

.taskInfoCard__prazo {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
}
</style>