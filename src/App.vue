<template>
  <Header />
  <SubHeader />
  <Table />
</template>

<script>
import Header from "./components/Header.vue";
import SubHeader from "./components/SubHeader.vue";
import Table from "./components/Table.vue";

export default {
  name: "App",
  components: {
    Header,
    SubHeader,
    Table,
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Karla:wght@300;400;500;600&family=Pacifico&display=swap');

* {
  margin: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
