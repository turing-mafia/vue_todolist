<template>
  <div class="home">
    <Addtodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
// @ is an alias to /src
import Todos from '@/components/Todos';
import Addtodo from '@/components/Addtodo';
import axios from 'axios';

export default {
  name: 'home',
  components: {
    Todos, Addtodo
  },
  data(){
    return {
      todos :[]
    }
  },
  methods:{
    deleteTodo(id){
      /* eslint-disable no-console,no-unused-vars */
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => this.todos=this.todos.filter(todo => todo.id !== id))
        .catch(err => console.log(err));

      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo){
      const {title, completed} = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos',{
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));

      //this.todos = [...this.todos, newTodo];
    }
  },
  created(){
      /* eslint-disable no-console */
      axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
        .then(res => this.todos=res.data)
        .catch(err => console.log(err));
    }
}
</script>
