<template>
  <div class="container" id="us-data-container">
    <h2>Covid19 US Data Table</h2>
    <div
      class="spinner-border text-secondary"
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
        id="us-data"
      >
        <thead id="us_table_header">
          <tr>
            <th scope="col">State</th>
            <th scope="col">Total Cases</th>
            <th scope="col">Total Deaths</th>
            <th scope="col" v-if="altApi === false">Total Recovered</th>
            <th v-else>Last Updated</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="stateObj in states" v-bind:key="stateObj.state">
            <th scope="row">{{ stateObj.state }}</th>
            <td>{{ stateObj.positive }}</td>
            <td>{{ stateObj.death }}</td>
            <td scope="col" v-if="altApi === false">
              {{ stateObj.recovered }}
            </td>
            <td v-else>{{ stateObj.updated }}</td>
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
  name: "Covid19USDataTable",
  data: function () {
    return {
      states: "",
      loading: false,
      errored: false,
      errorDetails: "unknown server error",
      altApi: false,
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
        .get(covid19api.usData)
        .then((res) => {
          this.states = res.data;
        })
        .catch((error) => {
          console.log(`initial request error ${error}`);
          this.makeAlternateAPIRequest();
        });
    },
    makeAlternateAPIRequest: function () {
      this.loading = true;
      axios
        .get(covid19api.altUSData)
        .then((res) => {
          res.data.forEach((obj) => {
            obj.positive = obj.case;
            delete obj.case;
          });
          this.altApi = true;
          this.states = res.data;
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
    states: function () {
      setTimeout(() => {
        // for now we use jquery.
        // TODO: find a better vue-dataTable
        // component and eliminate jquery.
        window.$("#us-data").DataTable({
          responsive: true,
          pageLength: 10,
        });
      }, 50);
    },
  },
};
</script>

<style>
#us-data-container h2 {
  margin-top: 5%;
}
#us_table_header {
  background: #dc3545bf;
}
#us-data-container {
  margin-left: 50px;
}
</style>
