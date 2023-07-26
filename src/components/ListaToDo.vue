<template>
    <div class="container-fluid contenedorT">
        <div class="row">
            <h1>LISTA DE TAREAS</h1>
            <div class="row justify-content-center">
                <div class="col-5">
                    <div class="input-group">
                        <input class="form-control" type="text" v-model="nuevaTarea" @keyup.enter="agregarTarea"
                            placeholder="AÑADIR TAREA" />
                    </div>
                </div>
                <div class="col-2 md">
                    <button type="button" @click="agregarTarea" class="btn btn-success botonancho">
                        AGREGAR
                    </button>
                </div>
            </div>
        </div>

        <div class="row rowLi d-flex justify-content-center">
            <div class="col-10">
                <div class="lista-tareas">
                    <div class="p-3 border d-flex justify-content-between" v-for="(tarea, index) in tareas" :key="index">
                        <span :class="{ 'tarea-realizada': tarea.realizada }">
                            {{ tarea.texto }}
                        </span>
                        <div class="d-flex justify-content-end">
                            <button type="button" class="btn btn-success botonS" @click="marcarRealizada(index)">
                                <img src="../assets/paloma.svg" />
                            </button>
                            <button type="button" class="btn btn-warning botonS" @click="editarTarea(index)">
                                <img src="../assets/lapiz.svg" />
                            </button>
                            <button type="button" class="btn btn-danger botonS" @click="eliminarTareaConfirmado">
                                <img src="../assets/cruz.svg" />
                            </button>
                            <div :class="showModal" v-show="showModal = false">
                                <div class="modal-content">
                                    <p>eliminar</p>
                                    <div>
                                        <button class="btn-si" @click="eliminarTareaConfirmado">SI</button>
                                        <button class="btn-no" @click="showModal = false">NO</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// Variables
const nuevaTarea = ref('');
const tareas = ref([]);
const showModal = ref(true);

// almacenar el índice de la tarea que se está editando
//const tareaEditandoIndex = ref(-1);

// almacenar el texto editado de la tarea
//const nuevaTextoTarea = ref('');
// Hooks
onMounted(() => {
    obtenerTareasDeLocalStorage();
});

// Métodos
const agregarTarea = () => {
    if (nuevaTarea.value.trim() !== '' && !tareas.value.some(tarea => tarea.texto === nuevaTarea.value.trim())) {
        tareas.value.push({ texto: nuevaTarea.value.trim(), realizada: false });
        guardarTareasEnLocalStorage();
        nuevaTarea.value = '';
    }
};

const marcarRealizada = (index) => {
    tareas.value[index].realizada = !tareas.value[index].realizada;
    guardarTareasEnLocalStorage();
};

const editarTarea = (index) => {
    const nuevaTexto = prompt('Editar tarea:', tareas.value[index].texto);
    if (nuevaTexto !== null && nuevaTexto.trim() !== '') {
        tareas.value[index].texto = nuevaTexto;
        guardarTareasEnLocalStorage();
    }
};

// Método para eliminar una tarea después de confirmar
const eliminarTareaConfirmado = (index) => {
    tareas.value.splice(index, 1);
    guardarTareasEnLocalStorage();
};

const obtenerTareasDeLocalStorage = () => {
    const tareasLocalStorage = localStorage.getItem('tareas');
    if (tareasLocalStorage) {
        tareas.value = JSON.parse(tareasLocalStorage);
    }
};
</script>

<style scoped>
.botonancho {
    width: 250px;
}

.contenedorT {
    margin-top: 100px;
}

.rowLi {
    margin-top: 20px;
}

.botonS {
    margin-right: 10px
}

/* Estilo para subrayar las tareas realizadas */
.tarea-realizada {
    text-decoration: line-through;
}

.lista-tareas {
    max-height: 400px;

    overflow-y: auto;

    border-radius: 5px;

    padding: 10px;


}

.showModal {
    position: fixed;
    /* Stay in place */
    z-index: 1;
    /* Sit on top */
    padding-top: 100px;
    /* Location of the box */
    left: 0;
    top: 0;
    width: 100%;
    /* Full width */
    height: 100%;
    /* Full height */
    overflow: auto;
    /* Enable scroll if needed */
    background-color: rgb(0, 0, 0);
    /* Fallback color */
    background-color: rgba(0, 0, 0, 0.4);
    /* Black w/ opacity */
}

.btn-si {
    background-color: green;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
}

.btn-no {
    background-color: red;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
}
</style>