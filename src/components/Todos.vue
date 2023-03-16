<template>
  <input class="qq" type="text" placeholder="Name..." v-model="title">
  <input type="submit" @click="addTodo" value="Add" class="add-btn" v-if="!isUpdate">
  <input type="submit" @click="" value="Update" class="add-btn" v-else>
  <Search class="qqq" @search-todo="searchTodo"/>
  <p v-for="todo in todos "
     :key="todo.id"
     :class="['todo-item', todo.completed ? 'is-completed' : '']"
     class="todos"
     >
    <input type="checkbox"
           :checked="todo.completed"
           @change="markCompleted(todo.id)"/>
    {{ todo.title }}
    <button class="del-btn" @click="deleteTodo(todo.id)">Delete</button>
    <button class="del-btn" @click="getTitle(todo.title)">Edit</button>
  </p>
</template>
<script>
import '../component-css/todos.css'
import {ref} from 'vue'
import axios from 'axios'
import Search from "./Search.vue";

export default {
  name: "Todos",
  components: {Search},
  setup() {
    const todos = ref([])
    const title = ref('')
    const search = ref('')
    const isUpdate = ref(false)
    const getAll = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
        todos.value = res.data
      } catch (e) {
        console.log(e)
      }
    }
    getAll()
    const addTodo = async (event) => {
      try {
        event.preventDefault();
        const newTodo = {
          title: title.value,
          complete: false
        }
        const res = await axios.post('https://jsonplaceholder.typicode.com/todos', newTodo)
        todos.value.push(res.data)
        console.log(res.data)
        title.value = ''
      } catch (e) {
        console.log(e)
      }
    }
    const searchTodo = async data => {
      search.value = data
      todos.value = todos.value.filter((todos) =>
          todos.title.includes(search.value.title)
      )
    }
    const deleteTodo = id => {
      todos.value = todos.value.filter(todo => todo.id !== id)
    }
    const markCompleted = id => {
      console.log(id)
      todos.value = todos.value.map((todo) => {
        if (todo.id === id) todo.completed = !todo.completed
        return todo
      })
    }
    const getTitle = (data) => {
      title.value = data
      isUpdate.value = true
    }
    const update = () => {

    }

    return {
      todos,
      title,
      addTodo,
      searchTodo,
      deleteTodo,
      markCompleted,
      getTitle,
      isUpdate
    }
  }
}
</script>

