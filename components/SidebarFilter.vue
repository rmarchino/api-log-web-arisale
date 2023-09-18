<template>
  <v-navigation-drawer 
    app
    width="400"
    :value="drawer"
    color="grey"
    class="container__form"
  >
    <v-btn @click="toggleDrawer" class="mb-5" color="button__cerrar">X</v-btn>
    <h2 class="mb-3">Búsqueda avanzada</h2>
    <v-form>
      <v-text-field
        v-model="idCompany" 
        label="idCompany" 
        clearable 
        outlined
        color="grey lighten-4"
      ></v-text-field>
      <v-text-field 
        v-model="idDevice" 
        label="idDevice" 
        clearable 
        outlined
        color="grey lighten-4"
      ></v-text-field>
      <v-text-field 
        v-model="endpoint" 
        label="endpoint" 
        clearable 
        outlined
        color="grey lighten-4"
      ></v-text-field>
      <v-col cols="12">
        <v-menu
              ref="startDateMenu"
              v-model="startDateMenu"
              :close-on-content-click="false"
              :nudge-width="200"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="dateRange"
                  label="Rango de Fechas"
                  prepend-icon="mdi-calendar"
                  readonly
                  clearable
                  outlined
                  v-on="on"
                ></v-text-field>
              </template>
              
              <v-date-picker
                v-model="dateRange"
                range
                no-title
                scrollable
                color="light-blue lighten-1"
              >
                <v-btn color="light-blue lighten-1" @click="saveDateRange"
                  >Save</v-btn
                >
                <v-btn color="deep-orange accent-2" @click="cancelDateRange"
                  >Cancel</v-btn
                >
              </v-date-picker>
        </v-menu>
      </v-col>

      <v-btn width="100%" class="button__search" @click="search">Buscar</v-btn>
    </v-form>
  </v-navigation-drawer>
</template>
  
<script>
import { mapActions, mapGetters, mapState } from  'vuex'

export default {
  props:['drawer', 'dateRange'],
  data() {
    return {
      loadin: false,
      startDateMenu: false,
      endDateMenu: false,
    }
  },
  computed: {
    ...mapState(['state']),

    idCompany: {
      set(val) {
        this.$store.commit('setIdCompany', val)
      },
      get() {
        return this.$store.state.searchData.idCompany
      },
    },
    idDevice: {
      set(val) {
        this.$store.commit('setIdDevice', val)
      },
      get() {
        return this.$store.state.searchData.idDevice
      }
    },
    startDate: {
      set(val) {
        this.dateRange[0] = val // Actualiza la fecha de inicio del rango
      },
      get() {
        return this.dateRange[0] // Obtiene la fecha de inicio del rango
      },
    },
    endDate: {
      set(val) {
        this.dateRange[1] = val // Actualiza la fecha de fin del rango
      },
      get() {
        return this.dateRange[1] // Obtiene la fecha de fin del rango
      },
    },
    endpoint: {
      set(val) {
        this.$store.commit('setEndpoint', val)
      },
      get() {
        return this.$store.state.searchData.endpoint
      }
    }
  },
  methods: {
    toggleDrawer() {
      this.$emit('toggleDrawer');
    },
    cancelDateRange() {
      this.startDateMenu = false
      this.dateRange = [this.startDate, this.endDate]
    },
    saveDateRange() {
      this.startDateMenu = false
      this.$emit('update:dateRange', { startDate: this.startDate, endDate: this.endDate });
      // this.$store.commit('setDateRange', { startDate: this.startDate, endDate: this.endDate });
    },

    async search() {
      try {
        // await this.$store.dispatch('searchData')
        console.log('Buscando...')

      } catch (error) {
        console.error('Error al obtener datos:', error)
      } finally {
        this.loading = false
      }
    }
  },
};
</script>
  
<style>
h2 {
  color: #d7d7d7;
}
.container__form {
  padding: 20px;
}
.button__cerrar {
  background: #f64040 !important;
}
.company, .device, .endpoint {
  background: #c4a8aa;
}
.button__search{
  background-color: #24B2C9 !important;
}
</style>