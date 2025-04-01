<script setup>
  import { ref, defineEmits } from 'vue';
  import api from '../plugins/axios.js'
  import Swal from 'sweetalert2';

  const searchTitle = ref('');
  const dialog = ref(false);
  const menu = ref(false);
  const selectedDate = ref('');
  const title = ref('');
  const description = ref('');
  const status = ref('');
  const errorMessage = ref('');

  const statusOptions = [
    { text: 'Pendente', value: 'pending' },
    { text: 'Em andamento', value: 'inProgress' },
    { text: 'Concluído', value: 'completed' },
  ];

  const emit = defineEmits(['search']);

  const handleSearch = (event) => {
    if (event.key === 'Enter') {
      emit('search', searchTitle.value);
    }
  };

  const saveTask = () => {
    if (!title.value || !status.value) {
      errorMessage.value = '*Por favor, preencha todos os campos obrigatórios.';
      return;
    }

    const data = {
      titulo: title.value,
      descricao: description.value,
      status: status.value.value,
      prazo: selectedDate.value? formattToDate(selectedDate.value) : null,
    };

    api.post('/tasks', data)
    .then(() => {
      closeModal();

      Swal.fire({
        title: "Tarefa adicionada",
        text: "Sua tarefa foi adicionada ao quadro com sucesso!",
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
        text: "Sua tarefa não foi adicionada ao quadro devido algum erro. Tente novamente!",
        icon: "error",
        confirmButtonText: "OK",
      }).then(() => {
        location.reload();
      });
    })
  }

  const closeModal = () => {
    title.value = '';
    description.value = '';
    status.value = '';
    selectedDate.value = '';
    errorMessage.value = '';
    dialog.value = false;
  };

  function formattToDate(date) {
    const data = new Date(date);
    return data.toISOString().split('T')[0];
  }
</script>

<template>
  <div class="searchAddBar">
    <input v-model="searchTitle" @keydown="handleSearch" type="text" class="searchAddBar__input" placeholder="Pesquise por título..." />
    <button class="searchAddBar__button" @click="dialog = true">Adicionar</button>
  </div>

  <v-dialog v-model="dialog" max-width="1000">
    <v-card title="Adicionar tarefa" style="padding: 1.5rem;">
      <v-card-text>
        <v-column style="gap: 1.5rem;">
          <v-text-field v-model="title" clearable label="*Título" variant="outlined" required></v-text-field>
          <v-textarea v-model="description" clearable label="Descrição" variant="outlined"></v-textarea>

          <v-row>
            <v-col cols="6">
              <v-select
                v-model="status"
                :items="statusOptions"
                item-title="text"
                item-value="value"
                label="*Status"
                chips
                return-object="false"
              ></v-select>
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
        </v-column>

        <small v-if="errorMessage" style="color: red;">{{ errorMessage }}</small>
        <small v-else class="text-caption text-medium-emphasis">*indica um campo obrigatório</small>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text="Fechar" variant="plain" @click="closeModal"></v-btn>
        <v-btn style="background-color: var(--secondary-color); color: white;" text="Adicionar" variant="tonal" @click="saveTask"></v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<style scoped>
.searchAddBar {
  width: 80%;
  height: fit-content;
  margin: 1.5rem auto 0rem;
  background-color: var(--primary-color);
  padding: 1rem 1rem;
  border-radius: 1rem;
  display: flex;
  justify-content: space-between;
}

.searchAddBar__input {
  width: 40%;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 1rem;
  font-size: 1rem;
  background-color: white;
  outline: none;

  &&:focus {
    background-color: var(--secondary-color);
    color: white;

    &&::placeholder {
      color: white;
    }
  }

}

.searchAddBar__button {
  width: fit-content;
  padding: 1rem 1.5rem;
  border: 2px solid white;
  border-radius: 0.75rem;
  background-color: transparent;
  font-size: 1rem;
  font-weight: bold;
  color: white;
  cursor: pointer;
  transition: 0.1s;
}

.searchAddBar__button:hover {
  background-color: var(--secondary-color);
  color: white;
  border: 2px solid var(--secondary-color);
}
</style>