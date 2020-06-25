<template>
  <div class="container">
    <h2>Covid19 Global Data Table</h2>
    <table
      class="table table-striped table-bordered table-sm"
      cellspacing="0"
      id="global-data"
    >
      <thead id="global_header">
        <tr>
          <th scope="col">Location</th>
          <th scope="col">NewConfirmed</th>
          <th scope="col">TotalConfirmed</th>
          <th scope="col">NewDeaths</th>
          <th scope="col">TotalDeaths</th>
          <th scope="col">NewRecovered</th>
          <th scope="col">TotalRecovered</th>
        </tr>
      </thead>
      <tbody>
        <tr class="table-warning">
          <th scope="row">{{ dataType }}</th>
          <td>{{ summary.NewConfirmed }}</td>
          <td>{{ summary.TotalConfirmed }}</td>
          <td>{{ summary.NewDeaths }}</td>
          <td>{{ summary.TotalDeaths }}</td>
          <td>{{ summary.NewRecovered }}</td>
          <td>{{ summary.TotalRecovered }}</td>
        </tr>
        <tr v-for="countryObj in countries" v-bind:key="countryObj.Country">
          <th scope="row">{{ countryObj.Country }}</th>
          <td>{{ countryObj.NewConfirmed }}</td>
          <td>{{ countryObj.TotalConfirmed }}</td>
          <td>{{ countryObj.NewDeaths }}</td>
          <td>{{ countryObj.TotalDeaths }}</td>
          <td>{{ countryObj.NewRecovered }}</td>
          <td>{{ countryObj.TotalRecovered }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Covid19GlobalDataTable",
  data: function () {
    return {
      summary: "",
      countries: "",
      dataType: "#Global",
    };
  },
  // life cycle function
  created: function () {
    this.makeAPIRequest();
  },
  methods: {
    makeAPIRequest: function () {
      axios
        .get("https://api.covid19api.com/summary")
        .then((res) => {
          this.summary = res.data.Global;
          this.countries = res.data.Countries;
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => console.log("Api request is completed"));
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
#global_header {
  background: rgba(0, 123, 255, 0.5);
}
.container {
  margin-left: 50px;
}
</style>
