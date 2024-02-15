<template>
  <div>
    <div class="modal fade" :id="modalId" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
      aria-labelledby="staticBackdropLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="staticBackdropLabel">Modal Editar</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
              @click="closeModal"></button>
          </div>
          <div class="modal-body">
            <div class="d-flex justify-content-center">
              <input placeholder="Ingrese su tarea aquí" type="text" class="form-control w-50" v-model="editedTitle" />
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="editTask()">Editar</button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="closeModal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['modalId', 'taskId', 'valueTask'],
  data() {
    return {
      editedTitle: this.valueTask,
    };
  },
  methods: {
    async editTask() {
      try {
        const response = await axios.put(`http://localhost:3000/posts/${this.taskId}`, {
          id: this.taskId,
          title: this.editedTitle,
          active: true
        });
        console.log(response.data);
      } catch (error) {
        console.error('Error al editar:', error.message);
      }
      this.$emit('task-edited');
    },
    closeModal() {
      this.$emit('close-modal');
    }
  }
};
</script>
