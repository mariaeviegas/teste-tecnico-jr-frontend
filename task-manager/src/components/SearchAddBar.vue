<script setup>
  import { ref } from 'vue';

  const dialog = ref(false);
  const menu = ref(false);
  const selectedDate = ref('');

</script>

<template>
  <div class="searchAddBar">
    <input type="text" class="searchAddBar__input" placeholder="Pesquise por título..." />
    <button class="searchAddBar__button" @click="dialog = true">Adicionar</button>
  </div>

  <v-dialog v-model="dialog" max-width="1000">
    <v-card title="Adicionar tarefa" style="padding: 1.5rem;">
      <v-card-text>
        <v-column style="gap: 1.5rem;">
          <v-text-field clearable label="*Título" variant="outlined" required></v-text-field>
          <v-textarea clearable label="Descrição" variant="outlined"></v-textarea>

          <v-row>
            <v-col cols="6">
              <v-select :items="['Pendente', 'Em andamento', 'Concluído']" label="*Status" chips></v-select>
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
                    :value="selectedDate"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="selectedDate" @update:model-value="menu = false"></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
        </v-column>

        <small class="text-caption text-medium-emphasis">*indica um campo obrigatório</small>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text="Fechar" 
          variant="plain" @click="dialog = false"></v-btn>
        <v-btn style="background-color: var(--secondary-color); color: white;" text="Adicionar" variant="tonal" @click="closeModal"></v-btn>
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