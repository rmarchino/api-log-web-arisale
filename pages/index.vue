<template>
  <v-app class="FFFF">
    <Header :drawer="drawer" :overlay="overlay" @toggleDrawer="toggleDrawer" />
    <SidebarFilter :drawer="drawer" :dateRange.sync="dateRange" @toggleDrawer="toggleDrawer"/>

    <main>
          <v-row class="container-tabla">
            <v-col cols="12">
              <v-data-table
                :headers="headers"
                :items="items"
                :loading="loading"
                :width="tableWidth"
                disable-pagination
                hide-default-footer
                class="header-table"
              >
                <template v-slot:item="props">
                  <tr class="custom-row">
                    <td class="custom-cell">{{ props.item.id }}</td>
                    <td class="custom-cell">{{ props.item.issueDate | formatIssueDate }}</td>
                    <td class="custom-cell">{{ props.item.duration | formatDuration }}</td>
                    <td class="custom-cell">{{ props.item.establishmentName }}</td>
                    <td class="custom-cell">{{ props.item.endpoint }}</td>
                    <td class="custom-cell">{{ props.item.method }}</td>
                    <td class="custom-cell">{{ props.item.responseCode }}</td>
                    <td class="custom-cell">{{ props.item.versionName }}</td>
                    <td class="custom-cell">{{ props.item.startDate | formatStartDate }}</td>
                    <td class="custom-cell">{{ props.item.endDate | formatEndDate }}</td>
                    <td class="custom-cell">
                      <SeeMore 
                        :item="props.item"
                      />
                    </td>
                  </tr>
                </template>
              </v-data-table>
            </v-col>
          </v-row>
      <!-- <v-tabs-items v-model="selectedTab">
        <v-tab-item v-for="(tab, index) in tabs" :key="index">
        </v-tab-item>
      </v-tabs-items> -->
      <v-row class="mt-5">
        <v-col class="d-flex" cols="12" sm="6">
          <v-btn
            @click="previousPage"
          >Previous</v-btn>
          <v-btn
            @click="nextPage"
          >Next</v-btn>
          <v-select
            v-model="perPage"
            :items="elementPagination"
            @input="changePerPage"
            label="Elementos por página"
            dense
            solo
            class="element__pagination"
          ></v-select>
          <div class="total__doc">Total de documentos: {{ docsCount }}</div>
        </v-col>
      </v-row>
    </main>
  </v-app>
</template>

<script>
import Header from '~/components/ui/Header.vue';
import SidebarFilter from '~/components/SidebarFilter.vue';
import SeeMore from '~/components/SeeMore.vue';

import { format } from 'date-fns';

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
      overlay: false,
      drawer: false,
      loading: false,
      sortable: false,
      align: 'start',
      tableWidth: '100%',
      elementPagination: [7, 10, 15, 25, 50, 100],
      dateRange: [null, null],
    }
  },

  filters: {
    formatStartDate(value) {
      if (!value) return ''
      return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
    },
    formatEndDate(value) {
      if (!value) return ''
      return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
    },
    formatDuration(value) {
      if (!value) return ''
      return value / 1000 + 'seg'
    },
    formatIssueDate(value) {
      if (!value) return ''
      return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
    },
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
      this.overlay = !this.overlay
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
.container-tabla {
  padding: 10px !important;
}
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
.total__doc {
  color: #545d61;
}
.element__pagination {
  color: #545d61;
}

/**Tabla */
.header-table{
  /* background-color: #b4b4b4 !important; */
  color: #545d61 !important;
}
.custom-cell {
  background-color: #fff !important;
}

.v-data-table > .v-data-table__wrapper > table > tbody > tr > td > .custom-cell {
  background: #fff !important;
}


</style>