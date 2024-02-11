<template>
  <div id="app">
    <Todo msg="Todo List"/>
  </div>
</template>

<script>
import axios from 'axios';
import Todo from './components/Todo.vue'

export default {
  name: 'App',
  data() {
    return {
      datos: null
    }
  },
  components: {
    Todo
  },
  methods:{
    async peticion (){

      try {
        let Servidor = await axios.get('https://jsonplaceholder.typicode.com/users');
        console.log(JSON.stringify(Servidor.data, undefined, 3));
        this.datos = Servidor;
      } catch (error) {
        console.log(error.message)
      }

    }, 
    getUsers() {
      axios({
        url: 'https://jsonplaceholder.typicode.com/users',
        method: 'GET',
      }).then((Respuesta) => console.log(Respuesta.data))
        .catch((error) => console.log(error.message));
    }
  },
  mounted(){
    this.getUsers();
  }
}
</script>

<style>
#app {
 color: rgb(56, 56, 56);
 text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
</style>
