<template>
  <div class="todo" :class="{ completed: todo.completed }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ todo.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditTodo', params: { id: todo.id }}">
          <span class="material-icons">edit</span>
        </router-link>
        <span @click="deleteTodo" class="material-icons">delete</span>
        <span @click="toggleComplete" class="material-icons tick">done</span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ todo.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  data() {
    return {
      showDetails: false,
      uri: 'http://localhost:3000/todos/' + this.todo.id
    }
  },
  methods: {
    deleteTodo() {
      fetch(this.uri, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.todo.id))
        .catch(err => console.log(err))
    },
    toggleComplete() {
      fetch(this.uri, { 
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ completed: !this.todo.completed })
      }).then(() => {
        this.$emit('complete', this.todo.id)
      }).catch(err => console.log(err))
    }
  },
}
</script>

<style>
  .todo {
    margin: 20px auto;
    background: #FFFFFF;
    padding: 10px 20px;
    border-radius: 5px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #e90074
  }
  
  .todo.completed {
    border-left-color: #00ce89;
  }
  
  .todo.completed .tick {
    color: #00ce89;
  }
  
  h3 {
    cursor: pointer;
  }
  
  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #BBBBBB;
    cursor: pointer;
  }
  
  .material-icons:hover {
    color: #888888;
  }
</style>