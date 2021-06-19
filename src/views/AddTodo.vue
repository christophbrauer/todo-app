<template>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input v-model="title" type="text" required>
    <label>Details</label>
    <textarea v-model="details" required></textarea>
    <button>Add Todo</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: ""
    }
  },
  methods: {
    handleSubmit() {
      let todo = {
        title: this.title,
        details: this.details,
        completed: false
      }
      
      fetch('http://localhost:3000/todos', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(todo)
      }).then(() => {
        this.$router.push('/')
      }).catch(err => console.log(err))
    }
  }
}
</script>

<style>
  form {
    background: #FFFFFF;
    padding: 20px;
    border-radius: 10px;
  }
  
  label {
    display: block;
    color: #BBBBBB;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
  }
  
  input, textarea {
    padding: 10px;
    border: 0;
    width: 100%;
    box-sizing: border-box;
  }
  
  input {
    border-bottom: 1px solid #DDDDDD;
  }
  
  textarea {
    border: 1px solid #DDDDDD;
    height: 100px;
  }
  
  form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: #FFFFFF;
    padding: 10px;
    border: 0;
    border-radius: 5px;
    font-size: 16px;
  }
</style>