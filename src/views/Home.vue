<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos"
    v-on:del-todo="deleteItem"
    v-on:todo-change="changeItem"
    />
  </div>
</template>


<script>
import Todos from '../components/Todos'
import AddTodo from '../components/AddTodo'
import axios from 'axios'
export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos : []
    }
  },
  methods: {
    deleteItem(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then( this.todos = this.todos.filter(e=> e.id != id))
      .catch(err => console.log(err));
    },
    changeItem(id) {
      for (let i=0; i<this.todos.length; i++) {
        if (this.todos[i].id == id) {
          this.todos[i].completed = !this.todos[i].completed
        }
      }

    },
    addTodo(newTodo) {
      const {title, completed} = newTodo;
      axios.post("https://jsonplaceholder.typicode.com/todos", {
        title, 
        completed
      })
      .then(res => this.todos.push(res.data))
      .catch(err => console.log(err));
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=15')
    .then(res => this.todos = res.data)
    .catch(err=>console.log(err));
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background-color: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background-color: #666;
}
</style>
