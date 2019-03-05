<template lang="html">
  <div>
      <form v-on:submit='addTodo($event)'>
          <input type='text' placeholder='Enter Todo' v-model='newTodo' />
          <input type='submit' />
      </form>
    <ul>
      <li v-for='todo in todos' :key="todo._id">
          <input type='checkbox' @click='deleteTodo(todo._id)' />
        <span>{{todo.title}}</span>
      </li>
    </ul>
  </div>
</template>
<script>
import ToDoAPI from '@/services/ToDoAPI.js'
export default {
  data () {
    return {
      todos: [],
      newTodo: ''
    }
  },
  mounted () {
    this.loadTodos()
  },
  methods: {
    async loadTodos () {
      const response = await ToDoAPI.getToDos()
      this.todos = response.data
    },
    async addTodo (evt) {
      evt.preventDefault() // prevents the form's default action from redirecting the page
      console.log('evt', evt)
      console.log('new todo', this.newTodo)
      const response = await ToDoAPI.addTodo(this.newTodo)
      console.log('response--', response)
      this.todos.push(response.data)
      this.newTodo = '' // clear the input field
    },
    deleteTodo (todoID) {
      ToDoAPI.deleteTodo(todoID)
      //  remove the array element with the matching id
      this.todos = this.todos.filter(function (obj) {
        return obj._id !== todoID
      })
    }
  }
}
</script>
<style lang="css">
</style>
