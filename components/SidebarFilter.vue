<template>
    <v-navigation-drawer app :value="drawer" @input="drawer = !drawer">
      <v-form>
        <v-text-field label="Campo 1"></v-text-field>
        <v-text-field label="Campo 2"></v-text-field>
        <v-text-field label="Campo 3"></v-text-field>
        <v-menu>
          <template v-slot:activator="{ on }">
            <v-text-field label="Selecciona una fecha" v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="selectedDate"></v-date-picker>
        </v-menu>
        <v-btn @click="search">Buscar</v-btn>
      </v-form>
    </v-navigation-drawer>
</template>
  
<script>
export default {
  data() {
    return {
      drawer: false,
      selectedDate: null,
    };
  },
  methods: {
    search() {
      // Implementa la lógica de búsqueda aquí
    },
  },
};
  </script>
  