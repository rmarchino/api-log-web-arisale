<template>
  <v-card class="mx-auto overflow-hidden">
    <v-app-bar app color="grey lighten-5">
      <v-app-bar-nav-icon @click="toggleDrawer" class="menu__icon"/>
      <v-list-item-avatar width="120px" height="90px">
        <v-img
          src="https://ari-s3bucket.s3.amazonaws.com/assets/logo/arisale_logotipo.png"
        ></v-img>
      </v-list-item-avatar>

      <template v-slot:extension>
        <v-tabs align-with-title>
          <v-tab v-for="(tab, index) in tabs" :key="index" class="tab__title tab_container-border">
            {{ tab.name }}
          </v-tab>
        </v-tabs>
          <v-btn text class="btn__container">
            <span>Actualizar</span>
            <v-icon>mdi-history</v-icon>
          </v-btn>
      </template>
    </v-app-bar>
    <v-overlay v-model="overlay"></v-overlay>
  </v-card>
</template>

<script>
export default {
  props: ['drawer', 'overlay'],
  data() {
    return {
      tabs: [
        {
          name: 'Exceptions',
        },
        {
          name: 'Pinpad Events',
        },
        {
          name: 'Web Services',
        },
      ]
    }
  },
  methods: {
    toggleDrawer() {
      this.$emit('toggleDrawer')
    },
  },
}
</script>


<style scoped>
.menu__icon {
  color: #545d61 !important;
}
.tab_container-border{
  border: 1px solid #545d61;
  margin-bottom: 10px;
  border-radius: 5px;
}
.tab__title  {
  color: #545d61 !important;
}
.btn__container{
  color: #545d61;
  border: 1px solid #545d61;
}
</style>