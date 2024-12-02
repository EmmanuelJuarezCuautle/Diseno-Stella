<template>
  <div class="container mt-4">
    <h2 class="text-center">Gestión de Agencias</h2>

    <!-- Botón para agregar agencia -->
    <div class="text-right mb-3">
      <button class="btn btn-primary" @click="openForm">Agregar Agencia</button>
    </div>

    <!-- Formulario para crear/editar agencias -->
    <div v-if="showForm" class="card mb-4">
      <div class="card-body">
        <h4>{{ editMode ? 'Editar Agencia' : 'Agregar Agencia' }}</h4>
        <form @submit.prevent="saveAgency">
          <div class="form-group">
            <label for="name">Nombre de la Agencia</label>
            <input
              type="text"
              id="name"
              v-model="form.name"
              class="form-control"
              placeholder="Nombre de la agencia"
              required
            />
          </div>
          <button type="submit" class="btn btn-success mr-2">Guardar</button>
          <button type="button" class="btn btn-secondary" @click="cancelForm">Cancelar</button>
        </form>
      </div>
    </div>

    <!-- Tabla de agencias -->
    <table class="table table-bordered table-hover text-center">
      <thead class="thead-dark">
        <tr>
          <th>#</th>
          <th>Nombre</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(agency, index) in agencies" :key="agency.id">
          <td>{{ index + 1 }}</td>
          <td>{{ agency.name }}</td>
          <td>
            <button class="btn btn-warning btn-sm mr-2" @click="editAgency(agency)">Editar</button>
            <button class="btn btn-danger btn-sm" @click="deleteAgency(agency.id)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showForm: false,
      editMode: false,
      form: {
        id: null,
        name: ''
      },
      agencies: [
        { id: 1, name: 'Agencia Centro' },
        { id: 2, name: 'Agencia Norte' }
      ]
    };
  },
  methods: {
    openForm() {
      this.showForm = true;
      this.editMode = false;
      this.resetForm();
    },
    cancelForm() {
      this.showForm = false;
      this.resetForm();
    },
    resetForm() {
      this.form = {
        id: null,
        name: ''
      };
    },
    saveAgency() {
      if (this.editMode) {
        const index = this.agencies.findIndex((agency) => agency.id === this.form.id);
        if (index !== -1) {
          this.agencies.splice(index, 1, { ...this.form });
        }
      } else {
        const newAgency = {
          ...this.form,
          id: this.agencies.length ? Math.max(...this.agencies.map((a) => a.id)) + 1 : 1
        };
        this.agencies.push(newAgency);
      }
      this.showForm = false;
      this.resetForm();
    },
    editAgency(agency) {
      this.form = { ...agency };
      this.showForm = true;
      this.editMode = true;
    },
    deleteAgency(id) {
      if (confirm('¿Estás seguro de eliminar esta agencia?')) {
        this.agencies = this.agencies.filter((agency) => agency.id !== id);
      }
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}
.card {
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
