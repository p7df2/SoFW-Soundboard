<template>
  <CsvEditor />
</template>

<script>
import CsvEditor from "./components/CsvEditor.vue";

export default {
  name: "App",
  components: {
    CsvEditor,
  },
};
</script>

<style>
:root {
  --app-green: #10bd6a;
  --ui-blue: #0c9aa4;
  --ui-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  --ui-blue-hover: #107e86;
  background: var(--app-green);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  max-width: 100vw;
}
</style>
