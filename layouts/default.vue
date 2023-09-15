<template>
  <v-app>
    <!-- Header -->
    <v-app-bar dark fixed app color="grey lighten-1">
      <!-- <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-list-item-avatar width="120px" height="90px">
        <v-img
          src="https://ari-s3bucket.s3.amazonaws.com/assets/logo/arisale_logotipo.png"
        ></v-img>
      </v-list-item-avatar> -->

      <!-- Tabs -->
      <!-- <template v-slot:extension>
        <v-tabs align-with-title>
          <v-tab>Exceptions</v-tab>
          <v-tab>Pinpad Events</v-tab>
          <v-tab>Web services</v-tab>
        </v-tabs>
      </template> -->
      <Header />
    </v-app-bar>

    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

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
        <v-btn @click="search" width="100%" color="#68c6e8"> Buscar </v-btn>
      </v-form>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import Header from '../components/ui/Header.vue'

export default {
  name: 'DefaultLayout',
  components: {
    Header,
  },
  data() {
    return {
      selectedDate: null,
      drawer: false,
    }
  },
  methods: {
    search() {
      //IN..
    },
  },
}
</script>

<style>
.container__form {
  padding: 1rem;
}
</style>
