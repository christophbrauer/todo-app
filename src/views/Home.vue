<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="todos.length">
      <div v-for="todo in filteredTodos" :key="todo.id">
        <SingleTodo :todo="todo" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleTodo from '../components/SingleTodo.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {
    SingleTodo,
    FilterNav
  },
  data () {
    return {
      todos: [],
      current: 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/todos')
    .then(res => res.json())
    .then(data => this.todos = data)
    .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id
      })
    },
    handleComplete(id) {
      let p = this.todos.find(todo => {
        return todo.id === id
      })
      p.completed = !p.completed
    }
  },
  computed: {
    filteredTodos() {
      if (this.current === 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      if (this.current === 'ongoing') {
        return this.todos.filter(todo => !todo.completed)
      }
      return this.todos
    }
  }
}
</script>
