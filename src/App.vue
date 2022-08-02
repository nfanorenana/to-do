<template>
  <div id="app">
    <todo-app></todo-app>
  </div>
</template>

<script>
import TodoApp from "./components/TodoApp.vue";

export default {
  name: "App",
  components: {
    TodoApp,
  },
};
</script>

<style>
body {
  background: #f0ece3 !important;
}
</style>
