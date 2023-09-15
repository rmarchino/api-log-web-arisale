<template>
  <v-app-bar dark fixed app color="grey lighten-1">
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-list-item-avatar width="120px" height="90px">
          <v-img
              src="https://ari-s3bucket.s3.amazonaws.com/assets/logo/arisale_logotipo.png"
          ></v-img>
      </v-list-item-avatar>
  
      <template v-slot:extension>
          <v-tabs align-with-title>
              <v-tab>Exceptions</v-tab>
              <v-tab>Pinpad Events</v-tab>
              <v-tab>Web services</v-tab>
          </v-tabs>
      </template>
  </v-app-bar>
</template>

<script>
export default {
  props: ['drawer'],
  methods: {
      // Add your methods here if any
  }
}
</script>
