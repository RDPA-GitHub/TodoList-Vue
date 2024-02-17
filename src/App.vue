<template>
  <div id="app">
    <div class="d-flex justify-content-center">
      <Todo msg="Listados" />
      <div class="d-flex align-items-center mt-2">
        <button title="Mostrando Usuarios Estáticos" class="ms-3"
          :class="['btn', datos.length > 0 ? 'btn-secondary' : 'btn-success', 'fw-bold']" @click="ListaOculta()">
          ⚡
        </button>
      </div>
    </div>

    <div class="container">

      <div class="row mt-3">
        <div class="col-md-12">

          <div class="d-flex justify-content-center">
            <input @keyup.enter="Crear()" placeholder="ingrese su tarea aquí" type="text" class="form-control w-50"
              v-model="Tarea.title" />
          </div>

        </div>
      </div>

      <!-- JSON API -->
      <div class="row mt-3  ">
        <div class="col-md-12 table-responsive">
          <table v-if="datos.length > 0" class="table table-dark table-striped">
            <thead class="table-active">
              <tr class="text-center">

                <th>No.</th>
                <th>Nombre</th>
                <th>Nombre de Usuario</th>

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

      <!-- Listado de JSON Server -->
      <div class="row mt-3  ">
        <div class="col-md-12 table-responsive">
          <table class="table table-dark table-striped">
            <thead class="table-active">
              <tr class="text-center">

                <th>No.</th>
                <th>Nombre</th>
                <th>Estado</th>
                <th>Acciones</th>

              </tr>
            </thead>
            <tbody>
              <tr class="text-center" v-for="(value, index) in usuarios" :key="index">

                <td>{{ index + 1 }}</td>
                <td>{{ value.title }}</td>
                <td :class="[value.active ? 'text-success fw-bold' : 'text-danger fw-bold']">
                  {{ value.active ? 'Activo' : 'Inactivo' }}
                </td>

                <td>
                  <button class="btn btn-outline-warning me-1 fw-bold" data-bs-toggle="modal"
                    :data-bs-target="`#editModal-${value.id}`">
                    Edit
                  </button>
                  <button class="btn btn-outline-danger ms-1 fw-bold" data-bs-toggle="modal"
                    :data-bs-target="`#deleteModal-${value.id}`">
                    Delete
                  </button>
                </td>

              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>


    <div v-for="data in usuarios" :key="data.id">

      <!-- Modal Editar -->
      <modal-editar :modalId="`editModal-${data.id}`" :taskId="data.id" :valueTask="data.title" @task-edited="getUsers"
        @close-modal="Actualizar" />

      <!-- Modal Delete -->
      <modal-borrar :modalId="`deleteModal-${data.id}`" :taskId="data.id" :valueTask="data.title" @task-edited="getUsers"
      @close-modal="Actualizar" />

      <div class="modal fade" :id="`deleteModal-${data.id}`" data-bs-backdrop="static" data-bs-keyboard="false"
        tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="staticBackdropLabel">Modal Delete</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
                @click="Actualizar()"></button>
            </div>
            <div class="modal-body text-center">
              Estás seguro que quieres borrar <strong class="text-danger">"{{ data.title }}"</strong>?
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger fw-bold" data-bs-dismiss="modal"
                @click="Borrar(data.id)">Borrar</button>
              <button type="button" class="btn btn-secondary fw-bold" data-bs-dismiss="modal"
                @click="Actualizar()">Cerrar</button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import Todo from './components/Todo.vue';
import ModalEditar from './components/ModalEditar.vue';
import ModalBorrar from './components/ModalBorrar.vue';

export default {
  name: 'App',
  components: {
    Todo,
    ModalEditar,
    ModalBorrar
  },
  methods: {
    async peticion() {

      try {
        let Servidor = await axios.get(this.url);

        this.datos = Servidor.data;
        console.log(Servidor.data);
      } catch (error) {
        console.log(error.message)
      }

    },
    getUsers() {
      axios({
        url: this.api,
        method: 'GET'
      }).then((Respuesta) => {
        this.usuarios = Respuesta.data;
        console.log(JSON.stringify(this.usuarios, undefined, 3));
      })
        .catch((error) => console.log(error.message));
    },
    ListaOculta() {
      if (this.datos.length > 0) {
        this.datos = [];
      } else {
        this.peticion();
      }
    },
    async Borrar(id) {
      try {
        let response = await axios.delete(`${this.api}/${id}`);
        console.log(JSON.stringify(response.data, undefined, 3));
      } catch (error) {
        console.error('Error al eliminar:', error.message);
      }

      this.Actualizar();

    },
    async Crear() {
      console.log(this.Tarea);

      try {

        let request = this.Tarea;

        console.log(request);

        let Servidor = await axios.post(this.api, request);

        this.datos = Servidor.data;
        console.log(JSON.stringify(this.datos, undefined, 3));
      } catch (error) {
        console.log(error.message)
      }

      this.getUsers();
      this.Tarea.title = '';
    },
    Actualizar() {
      this.getUsers();
    }

  },
  data() {
    return {
      datos: [],
      usuarios: [],
      vEdit: '',
      indice: '',
      Tarea: {
        title: null,
        active: true
      },
      url: 'https://jsonplaceholder.typicode.com/users',
      api: 'http://localhost:3000/posts'
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
