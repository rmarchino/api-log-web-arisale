<template>
    <!-- Menu desplegable -->
    <v-navigation-drawer 
      v-model="drawer"
      absolute
      temporary
      color="grey"
      width="350"
    >
      <v-form class="container__form">
        <h1 class="mb-3">Búqueda Avanzada</h1>
        <v-text-field outlined label="idCompany"></v-text-field>
        <v-text-field outlined label="idDevice"></v-text-field>
        <v-text-field outlined label="endpoint"></v-text-field>

        <v-menu>
          <template v-slot:activator="{ on }">
            <v-text-field label="Selecciona una fecha" v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="selectedDate"></v-date-picker>
        </v-menu>
        <v-btn 
          @click="search"
          width="100%"
          color="#68c6e8"
        >
          Buscar
        </v-btn>
      </v-form>
    </v-navigation-drawer>
</template>


<script>
export default {
    data() {
        return {
            drawer: true,
            selectedDate: null,
        }
    },
    methods: {
        search() {
            //implementar...
        }
    }
}
</script>