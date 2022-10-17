<script>
import ProgressBar from './ProgressBar.vue';
import TotalProyectos from './TotalProyectos.vue';
  export default{
    data: () => ({
        proyecto: "",
        tipo: "",
        urgente: false,
        proyectos: [],
    }),
    methods: {
        registrarProyecto() {
            const proyecto = {
                proyecto: this.proyecto,
                tipo: this.tipo,
                urgente: this.urgente,
                completado: false,
            };
            this.proyectos.push(proyecto);

            localStorage.setItem("proyectos", JSON.stringify(this.proyectos));

                this.proyecto = "",
                this.tipo = "",
                this.urgente = false;
            //console.log(this.proyectos);
        },
        cambiarEstado(proyecto, campo) {
            //this.urgente= !this.urgente;
            //this.proyectos[id].urgente= !this.proyectos[id].urgente;
            proyecto[campo] = !proyecto[campo];
            //console.log(proyecto,campo)
        }
    },
    computed: {
        numeroProyectos() {
            return this.proyectos.length;
        },
        porcentaje() {
            let completados = 0;
            this.proyectos.map(proyecto => {
                if (proyecto.completado)
                    completados++;
            });
            //return (completados*100)/this.numeroProyectos;
            return (completados * 100) / this.numeroProyectos || 0;
        },
    },
    components: { ProgressBar, TotalProyectos },
    mounted() {
      this.proyectos=JSON.parse(localStorage.getItem("proyectos")) || [];
    }
};
</script>


<template>
    <!--pre> <Codigo javascript sin marcar error>
      {{proyecto}}
      {{tipo}}
      {{urgente}}
    <pre-->
      <div class="row">
        <div class="col-12 mb-4">
          <progress-bar :porcentaje="porcentaje" />
        </div>


        <div class="col-12 col-md-4">
          <form @submit.prevent="registrarProyecto">
            <div class="mb-3">
              <label  class="form-label">Proyecto</label>
              <input v-model.trim="proyecto" type="text" class="form-control" required/>
            </div>


            <div class="mb-3">
              <label class="form-label">Actividad</label>
              <select v-model.trip="tipo" class= "form-select" required>
                <option disabled select value=""> Selecciona un tipo de actividad </option>
                <option> Aplicaciones web con Vue.js</option>
                <option> Backed Services con Node.js</option>
                <option> App movil con React Native</option>
              </select>
            </div>

            <div class="mb-3 ">
              <input v-model="urgente" type="checkbox" class="form-check-input"/>
              <label class="form-check-label" for="exampleInputPassword1">Urgente</label>
            </div>

            <button type="submit" class="btn btn-primary">Guardar</button>
          </form>
        </div>
        <div class="col-12 col-md-8">
          <total-proyectos 
          :numeroProyectos="numeroProyectos" 
          :proyectos="proyectos" 
          :cambiarEstado="cambiarEstado" />
        </div>
      </div>
</template>