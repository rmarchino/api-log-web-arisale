<template>
  <v-navigation-drawer
    app
    width="350"
    :value="drawer"
    style="background: rgb(244, 244, 244); padding: 15px;"
  >
    <div class="button__flex-end">
      <v-btn @click="toggleDrawer" class="mb-5" color="button__cerrar">X</v-btn>
    </div>
    <h2 class="mb-3">Búsqueda avanzada</h2>

    <template>
      <v-form>
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="idCompany"
              label="idCompany"
              clearable
              outlined
              class="company-close"
            ></v-text-field>
          </v-col>

          <v-col cols="12">
            <v-text-field
              v-model="idDevice"
              label="idDevice"
              clearable
              outlined
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="endpoint"
              label="endpoint"
              clearable
              outlined
            ></v-text-field>
          </v-col>
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
              >
                <v-btn class="button__save" @click="saveDateRange"
                  >Save</v-btn
                >
                <v-btn class="button__cancel" @click="cancelDateRange"
                  >Cancel</v-btn
                >
              </v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="12" class="container__button-search">
            <v-btn class="button__search" @click="search">Buscar</v-btn>
          </v-col>
        </v-row>
      </v-form>
    </template>
  </v-navigation-drawer>
</template>

<script>
import { mapActions, mapGetters, mapState } from 'vuex'

export default {
  props: ['drawer'],
  data() {
    return {
      loadin: false,
      startDateMenu: false,
      endDateMenu: false,
      dateRange: [],
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
      },
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
      },
    },
  },
  methods: {
    toggleDrawer() {
      this.$emit('toggleDrawer')
    },
    cancelDateRange() {
      this.startDateMenu = false
      this.dateRange = [this.startDate, this.endDate]
    },
    saveDateRange() {
      this.startDateMenu = false
      this.$store.commit('setDateRange', { startDate: this.startDate, endDate: this.endDate });
    },

    async search() {
      try {
        await this.$store.dispatch('searchData')
        console.log('Buscando...')
      } catch (error) {
        console.error('Error al obtener datos:', error)
      } finally {
        this.loading = false
      }
    },
  },
}
</script>

<style>
h2 {
  color: #545d61;
  font-size: 1.25rem;
}

/** Button cerrar modal **/
.button__flex-end > .button__cerrar {
  width: 30px !important;
  min-width: 10px !important;
}

.theme--dark.v-btn.v-btn--has-bg  {
  background: none;
}
.button__flex-end {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  align-items: flex-end;
}
.v-btn--is-elevated  {
  box-shadow: none !important;
}

.button__flex-end .v-btn__content {
  font-size: 20px;
}

/** Button search **/
.button__search {
  background-color: #68c6e8 !important;
  font-size: 0.875rem !important;
}
.button__search .v-btn__content {
  color: #ffffff !important;
}

.container__button-search {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

/** Buttons save and cancel*/
.button__save {
  background: #68c6e8 !important;
  width: auto !important;
}
.button__cancel {
  background: #f26a62 !important;
  width: auto !important;
}
.theme--dark.v-picker__body {
  background: #fff;
  color: #545d61;
}
.v-application .primary--text {
  color: #68c6e8 !important;
}

/** Contenido text-field */
.theme--dark.v-input input, .theme--dark.v-input textarea {
  color: #545d61 !important;
  font-size: 0.875rem;
}
.theme--dark.v-label {
  color: #545d61 !important;
}
.v-input__control {
  color: #68c6e8 !important;
}
.mdi-calendar{
  color: #68c6e8 !important;
}

.v-text-field--outlined fieldset {
  border-color: #545d61 !important;
}

.v-icon--link  {
  color: #68c6e8 !important;
} 
.v-text-field--enclosed {
  color: #545d61 !important;
}

/***Sección fecha */
.v-menu__content {
  box-shadow: none !important;
}

.v-date-picker-header {
  background-color: #fff;
  color: #545d61;
}
.v-btn__content {
  color: #545d61;
}
.v-date-picker-table {
  background-color: #fff;
}
.theme--dark.v-date-picker-header .v-date-picker-header__value:not(.v-date-picker-header__value--disabled) button:not(:hover):not(:focus) {
    color: #545d61;
}
.theme--dark.v-date-picker-table th, .theme--dark.v-date-picker-table .v-date-picker-table--date__week {
  color: #545d61;
}
.v-application .accent {
  background-color: #68c6e8 !important;
}
.v-card__actions {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  background-color: #fff;
}

.theme--dark.v-card {
  background-color: #fff;
}

/** tabs */
.v-tabs-slider {
  background: white !important;
}
</style>
