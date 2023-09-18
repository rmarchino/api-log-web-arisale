<template>
  <v-container>
    <v-tabs-items>
      <v-tab-item>
        <v-row>
          <v-data-table 
            :headers="headers" 
            :items="items"
            :loading="loading"
            disable-pagination
            hide-default-footer
          >
            <template>
              <tr>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
                <td>Ejemplo 1</td>
              </tr>
            </template>
          </v-data-table>
        </v-row>
      </v-tab-item>
    </v-tabs-items>
  </v-container>
</template>
  
<script>
import { mapGetters, mapState, mapActions } from 'vuex';
// import { format } from 'date-fns';

export default {
  data() {
    return {
      headers: [
        { text: 'Issue Date', value: 'issueDate' },
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
      loading: false,
    };
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
    selectedTable: {
      set(val) {
        this.$store.commit('setSelectedTab', val)
      },
      get() {
        return this.$store.state.selectedTab
      },
    }
  }
};
</script>
  