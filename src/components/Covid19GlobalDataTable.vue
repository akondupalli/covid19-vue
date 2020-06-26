<template>
  <div class="container" id="global-data-container">
    <h2>Covid19 Global Data Table</h2>
    <div
      class="spinner-border text-primary"
      role="status"
      v-if="loading === true"
    >
      <span class="sr-only">Loading...</span>
    </div>
    <div class="error" v-else-if="errored === true">
      <p>{{ errorDetails }}</p>
    </div>
    <div v-else>
      <table
        class="table table-striped table-bordered table-sm"
        cellspacing="0"
        id="global-data"
      >
        <thead id="global_table_header">
          <tr>
            <th scope="col">Location</th>
            <th scope="col">New Confirmed</th>
            <th scope="col">Total Confirmed</th>
            <th scope="col">Total Deaths</th>
            <th scope="col">New Recovered</th>
            <th scope="col">Total Recovered</th>
          </tr>
        </thead>
        <tbody>
          <tr class="table-warning">
            <th scope="row">{{ dataType }}</th>
            <td>{{ summary.NewConfirmed }}</td>
            <td>{{ summary.TotalConfirmed }}</td>
            <td>{{ summary.TotalDeaths }}</td>
            <td>{{ summary.NewRecovered }}</td>
            <td>{{ summary.TotalRecovered }}</td>
          </tr>
          <tr v-for="countryObj in countries" v-bind:key="countryObj.Country">
            <th scope="row">{{ countryObj.Country }}</th>
            <td>{{ countryObj.NewConfirmed }}</td>
            <td>{{ countryObj.TotalConfirmed }}</td>
            <td>{{ countryObj.TotalDeaths }}</td>
            <td>{{ countryObj.NewRecovered }}</td>
            <td>{{ countryObj.TotalRecovered }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import covid19api from "../assets/json/covid19-api.json";
export default {
  name: "Covid19GlobalDataTable",
  data: function () {
    return {
      summary: "",
      countries: "",
      dataType: "#Global",
      loading: false,
      errored: false,
      errorDetails: "unknown server error",
    };
  },
  // life cycle function
  created: function () {
    this.makeAPIRequest();
  },
  methods: {
    makeAPIRequest: function () {
      this.loading = true;
      axios
        .get(covid19api.globalData)
        .then((res) => {
          this.summary = res.data.Global;
          this.countries = res.data.Countries;
        })
        .catch((error) => {
          console.log(error);
          this.errorDetails = error;
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
  watch: {
    // load the data table once
    // we get the data from API
    countries: function () {
      setTimeout(() => {
        // for now we use jquery.
        // TODO: find a better vue-dataTable
        // component and eliminate jquery.
        window.$("#global-data").DataTable({
          responsive: true,
          pageLength: 10,
        });
      }, 50);
    },
  },
};
</script>

<style>
#global_table_header {
  background: rgba(0, 123, 255, 0.5);
}
#global-data-container {
  margin-left: 50px;
}
</style>
