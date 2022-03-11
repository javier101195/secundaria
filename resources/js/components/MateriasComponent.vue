<template>
    <div>
        <h1 class="text-center">Materias</h1>
        <hr>

        <!-- Button trigger modal -->
        <button type="button" class="btn btn-primary my-4" data-bs-toggle="modal" data-bs-target="#exampleModal">
        Registrar Materia
        </button>

        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">{{tituloModal}}</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div class="form-group my-4">
                                <label for="exampleFormControlInput1">Nombre de la materia</label>
                                <input v-model="materia.nombre" name='nombre' type="name" class="form-control" id="nombre" placeholder="Español">
                            </div>
                            <div class="form-group my-4">
                                <label for="exampleFormControlInput1">Semestre</label>
                                <input v-model="materia.semestre" name="semestre" type="number" class="form-control" id="semestre" placeholder="1">
                            </div>
                            <div class="form-group my-4">
                                <label for="exampleFormControlInput1">No. Creditos</label>
                                <input v-model="materia.creditos" name="creditos" type="number" class="form-control" id="creditos" placeholder="1">
                            </div>
                            <div class="form-group my-4">
                                <label for="maestro">Selecciona un maestro</label>
                                <select v-model="materia.maestro_id" class="form-control" id="maestro" >
                                    <option name="maestro_id" v-for="maestro in maestros" :key="maestro.id" :value="maestro.id">{{maestro.nombre}}</option>
                                </select>
                            </div>
                        </form>                
                    </div>
                    
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                        <button @click="guardar" class="btn btn-primary" data-bs-dismiss="modal" >Guardar</button>
                        
                    </div>
                </div>
            </div>
        </div>       

        <form>
            <div class="form-group my-4">
                <label for="m">Selecciona un maestro</label>
                <select v-model="maestroSelec" class="form-control" id="m" >
                    <option v-for="m in maestros" :key="m.id" :value="m.id">{{m.nombre}}</option>
                </select>
            </div>
            <div class="col-6">
                <h2>Docente seleccionado:</h2>
                {{maestroSelec}}
            </div>

        </form>       

        <table class="table table-borderless align-middle">
            <thead class="table-dark ">
                <tr>
                  <!-- <th scope="col">#</th> -->
                  <th scope="col">Materia</th>
                  <th scope="col">Semestre</th>
                  <th scope="col">Creditos</th>
                  <th scope="col">Maestro</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="mtr in materias" :key="mtr.id" for="maestro">
                    <!-- <th scope="row">{{mtr.id}}</th> -->
                    <td>{{mtr.nombre}}</td>
                    <td>{{mtr.semestre}}</td> 
                    <td>{{mtr.creditos}}</td>
                    <td id="maestro">{{maestros[mtr.maestro_id-1].nombre}}</td>
                </tr>
            </tbody>
        </table>        
    </div>
</template>

<script>


export default {
    data() {
        return{
            materia:{
                nombre:'',
                semestre:'',
                creditos:'',
                maestro_id:'',                
            },
                        
            modal:0,
            tituloModal:'Registra una materia',
            maestroSelec: {},
            materias:[],
            maestros:[], // <-- La lista de docentes
            

        }
    },
    methods:{
        async listar(){
            const res=await axios.get('materias')
            this.materias=res.data;           
        },
        async listar2(){
            const resultado=await axios.get('maestros')
            this.maestros=resultado.data;
        },

        /* abrirModal(){
        this.modal=1;
        },

        cerrarModal(){
        this.modal=0;
        }, */

        /* eliminar(id){
            const res=await axios.delete('/materias/'+id);
            this.listar();            
        }, */

        async guardar(){
            const res3=await axios.post('/materias', this.materia);
            //alert("hola");
            
            this.listar();            
        },     
    },
    created(){
        this.listar();
        this.listar2();
    },        
}


</script>
<style>
.mostrar{
    display: list-item;
    opacity: 1;
    background: rgb(44, 38, 75, 0.849);
}

</style>