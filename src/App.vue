<template>
  <div id="app">
    <Covid19GlobalDataTable />
    <Covid19USDataTable />
  </div>
</template>

<script>
import Covid19GlobalDataTable from "./components/Covid19GlobalDataTable";
import Covid19USDataTable from "./components/Covid19USDataTable";

export default {
  name: "App",
  components: {
    Covid19GlobalDataTable,
    Covid19USDataTable,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
html {
  zoom: 0.55;
}
</style>
