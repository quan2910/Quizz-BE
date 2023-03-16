<template>
  <form @submit="searchTodo">
    <input type="text" placeholder="Name..." v-model="title">
    <input type="submit" value="Search" class="add-btn">
  </form>
</template>

<script>
import {ref} from "vue";

export default {
  name: "searchTodo",
  setup (props, context) {
    const title = ref('');
    const searchTodo = (event) => {
      event.preventDefault()
      const searchJob = {
        title: title.value
      }
      context.emit('search-todo', searchJob)
      title.value = ''
    }
    return{
      title: title,
      searchTodo: searchTodo
    }
  }
}
</script>
