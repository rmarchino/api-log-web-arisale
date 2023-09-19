<template>
  <v-app>
    <Header :drawer="drawer" @toggleDrawer="toggleDrawer" />
    <SidebarFilter :drawer="drawer" :dateRange.sync="dateRange" @toggleDrawer="toggleDrawer"/>

    <v-container fluid>
      <v-data-table
        :headers="headers"
        :items="items"
        :loading="loading"
        :width="tableWidth"
        disable-pagination
        hide-default-footer
      >
        <template v-slot:item="props">
          <tr>
            <td>{{ props.item.id }}</td>
            <td>{{ props.item.issueDate }}</td>
            <td>{{ props.item.duration }}</td>
            <td>{{ props.item.establishmentName }}</td>
            <td>{{ props.item.endpoint }}</td>
            <td>{{ props.item.method }}</td>
            <td>{{ props.item.responseCode }}</td>
            <td>{{ props.item.versionName }}</td>
            <td>{{ props.item.startDate }}</td>
            <td>{{ props.item.endDate }}</td>
            <td>
              <SeeMore 
                :item="props.item"
              />
            </td>
          </tr>
        </template>
      </v-data-table>
        <v-card>
          <v-col cols="12" class="container__button-flex">
            <v-btn
              color="#ABB2B9"
              class="button__previous"
              @click="previousPage"
              >Previous</v-btn
            >
            <v-btn 
              color="#ABB2B9" 
              class="button__next" 
              @click="nextPage"
              >Next</v-btn
            >
            <v-select
              v-model="perPage"
              :items="elementPagination"
              label="Elementos por página"
              @input="changePerPage"
              class="element__pagination"
            ></v-select>
            <div class="total__doc">Total de documentos: {{ docsCount }}</div>
          </v-col>
        </v-card>
    </v-container>
  </v-app>
</template>

<script>
import Header from '~/components/ui/Header.vue';
import SidebarFilter from '~/components/SidebarFilter.vue';
import SeeMore from '~/components/SeeMore.vue';


import { mapGetters, mapActions, mapState } from 'vuex'

export default {
  name: 'IndexPage',
  components: {
    Header,
    SidebarFilter,
    SeeMore,
  },
  data() {
    return {
      headers: [
        { text: 'ID', value: 'id', align:'start', sortable: false },
        { text: 'Issue Date', value: 'issueDate', align:'start', sortable: false },
        { text: 'Duration', value: 'duration' },
        { text: 'Name Establishment', value: 'establishmentName' },
        { text: 'Endpoint', value: 'endpoint' },
        { text: 'Method', value: 'method' },
        { text: 'Response Code', value: 'responseCode' },
        { text: 'Versión Name', value: 'versionName' },
        { text: 'StartDate', value: 'startDate' },
        { text: 'EndDate', value: 'endDate' },
        { text: 'Actions', value: '' },
      ],
      drawer: false,
      loading: false,
      tableWidth: '100%',
      elementPagination: [7, 10, 15, 25, 50, 100],
      dateRange: [null, null],
    }
  },
  computed: {
    ...mapGetters(['isSearchDataComplete']),
    ...mapState(['items', 'state', 'perPage', 'docsCount']),

    perPage: {
      get() {
        return this.$store.state.perPage
      },
      set(val) {
        this.$store.commit('setPerPage', val)
      },
    },
    selectedTab: {
      set(val) {
        this.$store.commit('setSelectedTab', val)
      },
      get() {
        return this.$store.state.selectedTab
      },
    },
  },
  methods: {
    ...mapActions(['nextPage', 'previousPage', 'changePerPage']),

    toggleDrawer() {
      this.drawer = !this.drawer
    }
  },
  async changePerPage() {
      try {
        await this.$store.dispatch('changePerPage', this.perPage)
      } catch (error) {
        console.log('Error al cambiar la cantidad de elementos por página:', error)
      }
    },
}
</script>


<style scoped>
.container__button-flex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  column-gap: 10px;
}
.button__previous,
.button__next {
  text-transform: capitalize;
  font-size: 14px !important;
}
</style>