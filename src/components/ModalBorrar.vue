<template>
  <div>

    <div class="modal fade" :id="modalId" data-bs-backdrop="static" data-bs-keyboard="false"
      tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="staticBackdropLabel">Modal Delete</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
            @click="closeModal"></button>
          </div>
          <div class="modal-body text-center">
            Estás seguro que quieres borrar <strong class="text-danger">"{{ this.deleteTitle || 'Vacío'}}"</strong>?
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-danger fw-bold" data-bs-dismiss="modal"
              @click="DeleteTask()">Borrar</button>
            <button type="button" class="btn btn-secondary fw-bold" data-bs-dismiss="modal"
            @click="closeModal">Cerrar</button>
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
      deleteTitle: this.valueTask,
      api: 'http://localhost:3000/posts'
    };
  },
  methods: {
    async DeleteTask() {
      try {
        let response = await axios.delete(`${this.api}/${this.taskId}`);
        if(response.status == 200) console.log(  this.deleteTitle  + ' fue eliminado con exito!');
      } catch (error) {
        console.error('Error al eliminar:', error.message);
      }
      this.$emit('task-deleted');
    },
    closeModal() {
      this.$emit('close-modal');
    }
  }
};
</script>