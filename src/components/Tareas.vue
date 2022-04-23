<template>
 <div>
   <div class="container-fluid">
    <div>
    <!-- Button trigger modal -->
      <b-button class="btn-primary" v-b-modal.dependencias>Agregar Dependencia</b-button>
      <b-button class="btn-primary" v-b-modal.trabajador>Agregar Trabajador</b-button>
      <b-button class="btn-primary" v-b-modal.tareas>Agregar Tareas</b-button>
      <b-modal id="dependencias" title="Crear Dependencia">
        <p class="my-4">
          <div class="form-group">
            <label>Dependencia</label>
            <input class="form-control" v-model="inputDependencia">
          </div>
        </p>
      </b-modal>      
      <b-modal id="trabajador" title="Crear Trabajador">
        <p class="my-4">
          <div class="form-group">
            <label>Nombres</label>
            <input class="form-control" v-model="inputTrabajador['nombres']">
            <label>Apellidos</label>
            <input class="form-control" v-model="inputTrabajador['apellidos']">
            <label>Cargo</label>
            <select class="form-control" v-model="inputTrabajador['cargo']">
              <option v-for="cargo in cargos" v-text="cargo.nombre"></option>
            </select>
            <label>Edad</label>
            <input class="form-control" v-model="inputTrabajador['edad']">
            <label>Dependencia</label>
            <select class="form-control" v-model="inputTrabajador['dependencia']">
              <option v-for="dependencia in dependencias" v-text="dependencia.nombre"></option>
            </select>
          </div>
        </p>
      </b-modal>
      <b-modal id="tareas" title="Crear Tareas">
        <p class="my-4">
          <div class="form-group">
            <label>Descripción</label>
            <input class="form-control" v-model="inputTarea['descripcion']">
            <label>Fecha Limite Entrega</label>
            <input class="form-control" v-model="inputTarea['fecha_limite']">
            <select class="form-control" v-model="inputTarea['trabajador']">
              <option v-for="trabajador in trabajadores" v-text="trabajador.name"></option>
            </select>
          </div>
        </p>
      </b-modal>
    </div>
  </div>
  <div class="row d-flex justify-content-center">
    <card v-for="tarea in tareas" :title="'Tarea '+tipoTarea+' | '+tarea.persona" :sub-title="'Fecha Limite'+data.fecha_limite" style="width:60%">
      <div>
        {{tarea.descripcion}}
      </div>
    </card>
  </div>
</div>
</template>
<script>

export default {
  name: "Tareas",
  props: ['tipoTarea'],
  data() {
    return {
      tareas: [],
      trabajadores: [],
      dependencias: [],
      inputDependencia: '',
      inputTrabajador: [],
      inputTarea: []
    };
  },
  methods: {
    listarTareas()
    {
      axios.get('http://retogeek-api.test/api/listarTareas/'+this.tipoTarea).then((response) => {
        this.tareas = response.data.data;
      }) 
    },
    listarTrabajadores()
    {
      axios.get('http://retogeek-api.test/api/user/').then((response) => {
        this.trabajadores = response.data.data;
      });

      axios.get('http://retogeek-api.test/api/listarTrabajadores/').then((response) => {
        this.trabajadores = response.data.data;
      })
    },
    listarDependencias()
    {
      axios.get('http://retogeek-api.test/api/listarDependencias/').then((response) => {
        this.dependencias = response.data.data;
      })  
    }
  },
  mounted(){
    console.log('sss', this.tipoTarea);
    this.listarTareas(this.tipoTarea);
    this.listarTrabajadores();
    this.listarDependencias();
  }
};
</script>
<style>
.form-control{
  border: 1px solid #dddbdb !important;
}
</style>
