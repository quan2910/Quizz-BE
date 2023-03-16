<template>
  <div id="app">
    <Header/>
    <Todos/>
  </div>
</template>
<script>
import Header from "./components/Header.vue";
import Todos from "./components/Todos.vue";
export default {
  name: 'App',
  components : {
    Header, Todos
  }
}
</script>