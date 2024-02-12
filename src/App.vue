<template>
  <div id="app">
    <div class="d-flex justify-content-center">
      <Todo msg="Listado" />
      <div class="d-flex align-items-center mt-2">
        <button title="Mostrando Usuarios Estáticos" class="ms-3" :class="['btn', datos.length > 0 ? 'btn-secondary' : 'btn-success', 'fw-bold']"
        @click="Actualizacion()">
        ⚡
      </button>
      </div>
    </div>
    
    <div class="container">
      <div class="row mt-3  ">
        <div class="col-md-12 table-responsive">
          <table v-if="datos.length > 0" class="table table-dark table-striped">
            <thead class="table-active">
              <tr class="text-center">

                <th>ID</th>
                <th>Nombre</th>
                <th>Alias </th>

              </tr>
            </thead>
            <tbody>
              <tr class="text-center" v-for="(value, index) in datos" :key="index">

                <td>{{ value.id }}</td>
                <td>{{ value.name }}</td>
                <td>{{ value.username }}</td>

              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="row mt-3  ">
        <div class="col-md-12 table-responsive">
          <table class="table table-dark table-striped">
            <thead class="table-active">
              <tr class="text-center">

                <th>ID</th>
                <th>Nombre</th>
                <th>Estado</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr class="text-center" v-for="(value, index) in usuarios" :key="index">

                <td>{{ value.id }}</td>
                <td>{{ value.title }}</td>
                <td :class="[value.active ? 'text-success fw-bold' : 'text-danger fw-bold']">
                  {{ value.active ? 'Activo' : 'Inactivo'}}
                </td>

                <td>
                  <button class="btn btn-outline-warning me-1 fw-bold">
                    Edit
                  </button>
                  <button class="btn btn-outline-danger ms-1 fw-bold">
                    Delete
                  </button>
                </td>

              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>




  </div>
</template>

<script>
import axios from 'axios';
import Todo from './components/Todo.vue'

export default {
  name: 'App',
  components: {
    Todo,
  },
  methods: {
    async peticion() {

      try {
        let Servidor = await axios.get('https://jsonplaceholder.typicode.com/users');

        this.datos = Servidor.data;
        console.log(Servidor.data);
      } catch (error) {
        console.log(error.message)
      }

    },
    getUsers() {
      axios({
        url: 'http://localhost:3000/posts',
        method: 'GET'
      }).then((Respuesta) => {
        this.usuarios = Respuesta.data;
        console.log(Respuesta.data);
      })
        .catch((error) => console.log(error.message));
    },
    Actualizacion() {
      if(this.datos.length > 0){
        this.datos = [];
      }else{
        this.peticion();
      }
    }
  },
  data() {
    return {
      datos: [],
      usuarios: []
    }
  },
  mounted() {
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
