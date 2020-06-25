<template>
  <div id="app">
    <Covid19GlobalDataTable />
  </div>
</template>

<script>
import Covid19GlobalDataTable from "./components/Covid19GlobalDataTable";

export default {
  name: "App",
  components: {
    Covid19GlobalDataTable,
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
</style>
