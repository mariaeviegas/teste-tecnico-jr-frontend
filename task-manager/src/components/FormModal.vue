<script setup>
import { ref } from 'vue';

defineProps({
  dialog: {
    type: Boolean,
    required: true,
  },
});

const emit = defineEmits(['update:dialog']);

const closeModal = () => {
  emit('update:dialog', false);
};
</script>

<template>
  <v-dialog v-model="dialog" max-width="1000">
    <v-card title="Adicionar tarefa" style="padding: 1.5rem;">
      <v-card-text>
        <v-column style="gap: 1.5rem;">
          <v-text-field clearable label="*Título" variant="outlined" required></v-text-field>
          <v-textarea clearable label="Descrição" variant="outlined"></v-textarea>

          <v-row style="gap: 1.5rem;">
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
                  ></v-text-field>
                </template>
                <v-date-picker @update:model-value="menu = false"></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
        </v-column>

        <small class="text-caption text-medium-emphasis">*indica um campo obrigatório</small>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text="Fechar" variant="plain" @click="closeModal"></v-btn>
        <v-btn style="background-color: var(--secondary-color); color: white;" text="Adicionar" variant="tonal" @click="closeModal"></v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>