<template>
 <div>
   <div class="container-fluid">
    <div class="d-flex justify-content-end">
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
            <input class="form-control" type="date" v-model="inputTarea.fecha_limite">
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
      <b-modal @hide="cancelarForm" id="cambiarEstado" title="Cambiar Estado">
        <p class="my-4">
          <div class="form-group">
            <label>Observaciones</label>
            <textarea class="form-control" v-model="inputEstado.observaciones"></textarea>
            <label>Fecha</label>
            <input type="date" class="form-control" v-model="inputEstado.fecha">
            <input type="hidden" class="form-control" v-model="inputEstado.id_tarea">
            <label>Estado</label>
            <select v-model="inputEstado.id_estado" class="form-control">
              <option value="2">Cumplida</option>
              <option value="3">Incumplida</option>
            </select>
            <br>
            <button class="btn btn-primary" @click="registrarEstado">Guardar</button>
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
    <card v-for="(tarea,index) in tareas" style="width:60%">
      <div>
        <div class="row">
          <div class="col col-md-6">
            <h5 class="card-title">{{'Tarea '+(index+1)+' - '+tarea.persona}}</h5>
          </div>
          <div class="col col-md-6" style="text-align:end">
            <b-button class="btn-primary" v-show="tipoTarea == 'pendiente'" v-b-modal.cambiarEstado @click="cambiaIdTarea(tarea.id)">Cambiar Estado</b-button>
          </div>
        </div>
        <div>
          {{tarea.descripcion}}
        </div>
        <div class="d-flex justify-content-end">
          <br>
          {{tarea.dependencia+' | '+tarea.fecha_limite}}
        </div>
      </div>
    </card>
  </div>
  
  <div class="d-flex justify-content-end">
    <template v-if="pagination.current_page>1">
      <p>Nota: La paginación se muestra después de 3 tareas</p>
    </template>
    <template>    
      <div class="form-group">
        <nav>
            <ul class="pagination">
                <li class="page-item" v-if="pagination.current_page > 1">
                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page - 1,buscar,criterio)">Ant</a>
                </li>
                <li class="page-item" v-for="page in pagesNumber" :key="page" :class="[page == isActived ? 'active' : '']">
                    <a class="page-link" href="#" @click.prevent="cambiarPagina(page,buscar,criterio)" v-text="page"></a>
                </li>
                <li class="page-item" v-if="pagination.current_page < pagination.last_page">
                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page + 1,buscar,criterio)">Sig</a>
                </li>
            </ul>
        </nav>
      </div>
    </template>
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
      inputEstado: {},
      offset : 3,
      host: 'http://localhost/retogeek-back/public',
      pagination : {
          'total' : 0,
          'current_page' : 0,
          'per_page' : 0,
          'last_page' : 0,
          'from' : 0,
          'to' : 0,
      },
      headers: {
        headers: {
          Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3RcL3JldG8tZ2Vla1wvcHVibGljXC9hcGlcL2NyZWFyVG9rZW4iLCJpYXQiOjE2NTA4MTE0NTYsImV4cCI6MTY1MDgxNTA1NiwibmJmIjoxNjUwODExNDU2LCJqdGkiOiI2TFE5ckU5Y1diUzBERFdoIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.zm6qq9TEi_uCgpRxd3O_NuyCzX5rnGB_mGTisLjYHcw',
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        }
      }
    };
  },
  computed:{ // Almacenamos en Chache la Paginación
    isActived: function(){
        return this.pagination.current_page;
    },
    //Calcula los elementos de la paginación
    pagesNumber: function() {
        if(!this.pagination.to) {
            return [];
        }
        
        var from = this.pagination.current_page - this.offset; 
        if(from < 1) {
            from = 1;
        }

        var to = from + (this.offset * 2); 
        if(to >= this.pagination.last_page){
            to = this.pagination.last_page;
        }  

        var pagesArray = [];
        while(from <= to) {
            pagesArray.push(from);
            from++;
        }
        return pagesArray;             

    }
},
  methods: {
    cambiarPagina(page,buscar,criterio){ // Metodo encargado de realizar el cambio de pagina
      //Actualiza la página actual
      this.pagination.current_page = page;
      //Envia la petición para visualizar la data de esa página
      this.listarTareas(page);
    },
    cancelarForm()
    {
      this.errores = [];
      this.inputDependencia = '';
      this.inputTrabajador = {};
      this.inputTarea = {};
      this.inputEstado = {};
    },
    cambiaIdTarea(id)
    {
      this.inputEstado.id_tarea = id;
    },
    registrarEstado()
    {
      axios.post(
          this.host+'/api/cambiarEstado/tareas',
          this.inputEstado,
          this.headers
        ).then((response) => {
          if(response.data.status)
          {
            this.$root.$emit('bv::hide::modal','cambiarEstado');
            this.listarDependencias();
            alert('Estado cambiado exitosamente');
            this.listarTareas(1);
          }
          else
          {
            this.errores = response.data.data;
          }
      }).catch((error) => {
        this.crearToken();
        this.registrarEstado();
      })  
    },
    registrarDependencia()
    {
      axios.post(
          this.host+'/api/guardar/dependencias',
          {
            "nombre": this.inputDependencia
          },
          this.headers
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
      }).catch((error) => {
        this.crearToken();
        this.registrarDependencia();
      })  
    },
    registrarTrabajador()
    {
      axios.post(
          this.host+'/api/guardar/trabajadores',
          this.inputTrabajador,
          this.headers
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
      }).catch((error) => {
        this.crearToken();
        this.registrarTrabajador();
      })  
    },
    registrarTarea(){
      axios.post(
          this.host+'/api/guardar/tarea',
          this.inputTarea,
          this.headers
        ).then((response) => {
          if(response.data.status)
          {
            this.$root.$emit('bv::hide::modal','tareas');
            this.listarTareas(1);
            alert('Tarea creada exitosamente');
          }
          else
          {
            this.errores = response.data.data;
          }
      }).catch((error) => {
        this.crearToken();
        this.registrarTarea();
      })      
    },
    listarTareas(page)
    {
      axios.get(
          this.host+'/api/listarTareas/'+this.tipoTarea+'?page=' + page,
          this.headers
        ).then((response) => {
          console.log(response);
          this.pagination = response.data.data.pagination;
          this.tareas = response.data.data.tareas.data;
      }).catch((error) => {
        this.crearToken();
        this.listarTareas(page);
      })  
    },
    listarTrabajadores()
    {
      axios.get(
          this.host+'/api/listarTrabajadores',
          this.headers
        ).then((response) => {
        this.trabajadores = response.data.data;
      }).catch((error) => {
        this.crearToken();
        this.listarTrabajadores();
      })  
    },
    listarDependencias()
    {
      axios.get(
          this.host+'/api/listarDependencias',
          this.headers
        ).then((response) => {
          this.dependencias = response.data.data;
      }).catch((error) => {
        this.crearToken();
        this.listarDependencias();
      })    
    },
    listarCargos()
    {
      axios.get(
          this.host+'/api/listarCargos',
          this.headers
        ).then((response) => {
          this.cargos = response.data.data;
      }).catch((error) => {
        this.crearToken();
        this.listarCargos();
      })    
    },
    crearToken()
    {
      axios.get(
          this.host+'/api/crearToken'
        ).then((response) => {
          if(response.data.status)
            this.headers.headers = {
              Authorization: 'Bearer '+response.data.token,
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json',
            };
          else
            alert('No se pudo crear el token');
      }).catch((error) => {
        this.crearToken();
      })    
    }
  },
  mounted(){
    //console.log('sss', this.tipoTarea);
    this.listarTareas(1);
    this.listarTrabajadores();
    this.listarDependencias();
    this.crearToken();
    this.listarCargos();
  }
};
</script>
<style>
.form-control{
  border: 1px solid #dddbdb !important;
}
</style>
