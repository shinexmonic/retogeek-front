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
      axios.get(
          'http://localhost/reto-geek/public/api/listarTareas',
          {
            headers: {
              Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA3MzIwNzYsImV4cCI6MTY1MDczNTY3NiwibmJmIjoxNjUwNzMyMDc2LCJqdGkiOiJMbmZTUnZJNDBRZ3V4NzdNIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.tvfp7kdgVGdEr2B0HIq_VbbLCpVRw52IviDB7B5NlBk',
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
        this.tareas = response.data.data;
      })
    },
    listarTrabajadores()
    {
      axios.get(
          'http://localhost/reto-geek/public/api/user',
          {
            headers: {
              Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA3MzE5MDEsImV4cCI6MTY1MDczNTUwMSwibmJmIjoxNjUwNzMxOTAxLCJqdGkiOiJ5OGlqSmdrNktwRURmczRiIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.Z9zkWDh40yz9bDMLwdWFjQKl70J2AU7tjEcuovtUnpw',
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
        this.trabajadores = response.data.data;
      });

      axios.get(
          'http://localhost/reto-geek/public/api/listarTrabajadores/',
          {
            headers: {
              Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA3MzE5MDEsImV4cCI6MTY1MDczNTUwMSwibmJmIjoxNjUwNzMxOTAxLCJqdGkiOiJ5OGlqSmdrNktwRURmczRiIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.Z9zkWDh40yz9bDMLwdWFjQKl70J2AU7tjEcuovtUnpw',
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
        this.trabajadores = response.data.data;
      })
    },
    listarDependencias()
    {
      axios.get(
          'http://localhost/reto-geek/public/api/listarDependencias/',
          {
            headers: {
              Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA3MzE5MDEsImV4cCI6MTY1MDczNTUwMSwibmJmIjoxNjUwNzMxOTAxLCJqdGkiOiJ5OGlqSmdrNktwRURmczRiIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.Z9zkWDh40yz9bDMLwdWFjQKl70J2AU7tjEcuovtUnpw',
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            },
            mode: 'no-cors',
          }
        ).then((response) => {
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
