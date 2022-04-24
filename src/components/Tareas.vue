<template>
 <div>
   <div class="container-fluid">
    <div>
    <!-- Button trigger modal -->
      <b-button class="btn-primary" v-b-modal.dependencias>Agregar Dependencia</b-button>
      <b-button class="btn-primary" v-b-modal.trabajador>Agregar Trabajador</b-button>
      <b-button class="btn-primary" v-b-modal.tareas>Agregar Tareas</b-button>
      <b-modal @hide="cancelarForm" id="dependencias" title="Crear Dependencia">
        <p class="my-4">
          <div class="form-group">
            <label>Dependencia</label>
            <input class="form-control" v-model="inputDependencia">
            <br>
            <button class="btn btn-primary" @click="registrarDependencia">Guardar</button>
            <br>
            <div v-show="Object.keys(errores).length > 0">
              <br>
              <div class="alert alert-danger">
                <ul>
                  <template v-for="error in errores">
                    <li v-for="mensaje in error" v-text="mensaje"></li>
                  </template>
                </ul>
              </div>
            </div>
          </div>
        </p>
      </b-modal>      
      <b-modal @hide="cancelarForm" id="trabajador" title="Crear Trabajador">
        <p class="my-4">
          <div class="form-group">
            <label>Nombres</label>
            <input class="form-control" v-model="inputTrabajador.nombres">
            <label>Apellidos</label>
            <input class="form-control" v-model="inputTrabajador.apellidos">
            <label>Cargo</label>
            <select class="form-control" v-model="inputTrabajador.id_cargo">
              <option v-for="cargo in cargos" :value="cargo.id">{{cargo.nombre}}</option>
            </select>
            <label>Edad</label>
            <input class="form-control" v-model="inputTrabajador.edad">
            <label>Dependencia</label>
            <select class="form-control" v-model="inputTrabajador.id_dependencia">
              <option v-for="dependencia in dependencias" :value="dependencia.id">{{dependencia.nombre}}</option>
            </select>
            <br>
            <button class="btn btn-primary" @click="registrarTrabajador">Guardar</button>
            <br>
            <div v-show="Object.keys(errores).length > 0">
              <br>
              <div class="alert alert-danger">
                <ul>
                  <template v-for="error in errores">
                    <li v-for="mensaje in error" v-text="mensaje"></li>
                  </template>
                </ul>
              </div>
            </div>
          </div>
        </p>
      </b-modal>
      <b-modal @hide="cancelarForm" id="tareas" title="Crear Tareas">
        <p class="my-4">
          <div class="form-group">
            <label>Descripción</label>
            <textarea class="form-control" v-model="inputTarea.descripcion"></textarea>
            <label>Fecha Limite Entrega</label>
            <input class="form-control" v-model="inputTarea.fecha_limite">
            <label>Trabajador</label>
            <select class="form-control" v-model="inputTarea.id_trabajador">
              <option v-for="trabajador in trabajadores" :value="trabajador.id">{{trabajador.nombres + ' ' + trabajador.apellidos}}</option>
            </select>
            <br>
            <button class="btn btn-primary" @click="registrarTarea">Guardar</button><br>
            <div v-show="Object.keys(errores).length > 0">
              <br>
              <div class="alert alert-danger">
                <ul>
                  <template v-for="error in errores">
                    <li v-for="mensaje in error" v-text="mensaje"></li>
                  </template>
                </ul>
              </div>
            </div>
          </div>
        </p>
      </b-modal>
    </div>
  </div>
  <br>
  <div class="row d-flex justify-content-center">
    <br>
    <card v-for="tarea in tareas" :title="'Tarea '+tipoTarea+' | '+tarea.persona" :sub-title="'Fecha Limite '+tarea.fecha_limite" style="width:60%">
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
      errores: [],
      inputDependencia: '',
      inputTrabajador: {},
      cargos: [],
      inputTarea: {},
      token: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA3NTgyNjUsImV4cCI6MTY1MDc2MTg2NiwibmJmIjoxNjUwNzU4MjY2LCJqdGkiOiJQektnTjlLcEJYNVhOeGZSIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.w68gZxOXry6apU_TfwC1m-gFD_SEpqqpqoaeAeQkHb4'
    };
  },
  methods: {
    cancelarForm()
    {
      this.errores = [];
      this.inputDependencia = '';
      this.inputTrabajador = {};
      this.inputTarea = {};
    },
    registrarDependencia()
    {
      axios.post(
          'http://localhost/reto-geek/public/api/guardar/dependencias',
          {
            "nombre": this.inputDependencia
          },
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
          if(response.data.status)
          {
            this.$root.$emit('bv::hide::modal','dependencias');
            this.listarDependencias();
            alert('Dependencia creada exitosamente');
          }
          else
          {
            this.errores = response.data.data;
          }
      })
    },
    registrarTrabajador()
    {
      axios.post(
          'http://localhost/reto-geek/public/api/guardar/trabajadores',
            this.inputTrabajador,
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
          if(response.data.status)
          {
            this.$root.$emit('bv::hide::modal','trabajador');
            this.listarTrabajadores();
            alert('Trabajador creado exitosamente');
          }
          else
          {
            this.errores = response.data.data;
          }
      })
    },
    registrarTarea(){
      axios.post(
          'http://localhost/reto-geek/public/api/guardar/tarea',
            this.inputTarea,
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
          if(response.data.status)
          {
            this.$root.$emit('bv::hide::modal','tareas');
            this.listarDependencias();
            alert('Tarea creada exitosamente');
          }
          else
          {
            this.errores = response.data.data;
          }
      })      
    },
    listarTareas()
    {
      axios.get(
          'http://localhost/reto-geek/public/api/listarTareas/'+this.tipoTarea,
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
          }
        ).then((response) => {
        this.tareas = response.data.data;
        console.log(this.tareas);
      })
    },
    listarTrabajadores()
    {
      axios.get(
          'http://localhost/reto-geek/public/api/listarTrabajadores',
          {
            headers: {
              Authorization: 'Bearer '+this.token,
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
          'http://localhost/reto-geek/public/api/listarDependencias',
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
            //mode: 'no-cors',
          }
        ).then((response) => {
          this.dependencias = response.data.data;
      })  
    },
    listarCargos()
    {
      axios.get(
          'http://localhost/reto-geek/public/api/listarCargos',
          {
            headers: {
              Authorization: 'Bearer '+this.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            }
            //mode: 'no-cors',
          }
        ).then((response) => {
          this.cargos = response.data.data;
      })  
    }
  },
  mounted(){
    //console.log('sss', this.tipoTarea);
    this.listarTareas();
    this.listarTrabajadores();
    this.listarDependencias();
    this.listarCargos();
  }
};
</script>
<style>
.form-control{
  border: 1px solid #dddbdb !important;
}
</style>
