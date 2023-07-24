<template>
    <div class="container-fluid contenedorT">
      <div class="row">
        <h1>LISTA DE TAREAS</h1>
        <div class="row justify-content-center">
          <div class="col-5">
            <div class="input-group">
              <input class="form-control" 
                     type="text" 
                     v-model="nuevaTarea" 
                     @keyup.enter="agregarTarea"
                     placeholder="AÑADIR TAREA" />
            </div>
          </div>
          <div class="col-2">
            <button type="button" 
                    @click="agregarTarea" 
                    class="btn btn-success botonancho">
              AGREGAR
            </button>
          </div>
        </div>
      </div>
      <div class="row rowLi d-flex justify-content-center">
        <div class="col-10 ">
          <div class="p-3 border d-flex justify-content-between" 
               v-for="(tarea, index) in tareas" :key="index">
            <span :class="{ 'tarea-realizada': tarea.realizada }">
              {{ tarea.texto }}
            </span>
            <div class="d-flex justify-content-end">
              <button type="button" class="btn btn-success botonS" 
                      @click="marcarRealizada(index)">
                <svg xmlns="http://www.w3.org/2000/svg" 
                     width="16" 
                     height="16" 
                     fill="currentColor"
                     class="bi bi-check2" viewBox="0 0 16 16">
                  <path d="M13.854 3.646a.5.5 0 0 1 0 .708l-7 7a.5.5 0 0 1-.708 0l-3.5-3.5a.5.5 0 1 1 .708-.708L6.5 10.293l6.646-6.647a.5.5 0 0 1 .708 0z" />
                </svg>
              </button>
              <button type="button" 
                      class="btn btn-warning botonS" 
                      @click="editarTarea(index)">
                <svg xmlns="http://www.w3.org/2000/svg" 
                     width="16" 
                     height="16" 
                     fill="currentColor"
                     class="bi bi-pencil-square" 
                     viewBox="0 0 16 16">
                  <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456l-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z" />
                  <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z" />
                </svg>
              </button>
              <button type="button" class="btn btn-danger botonS" 
                      @click="eliminarTarea(index)">
                <svg xmlns="http://www.w3.org/2000/svg" 
                     width="16" 
                     height="16" 
                     fill="currentColor"
                     class="bi bi-x" 
                     viewBox="0 0 16 16">
                  <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="modal fade" id="confirmationModal" tabindex="-1" aria-labelledby="confirmationModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="confirmationModalLabel">Confirmación</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            ¿Seguro que quieres finalizar la tarea?
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-success" @click="marcarRealizada">Sí</button>
            <button type="button" class="btn btn-danger" data-bs-dismiss="modal">No</button>
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
  
  // Variable para almacenar el índice de la tarea que se está editando
  const tareaEditandoIndex = ref(-1);
  
  // Variable para almacenar el texto editado de la tarea
  const nuevaTextoTarea = ref('');
  
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
  
  const guardarEdicionTarea = () => {
    const index = tareaEditandoIndex.value;
    if (index !== -1 && nuevaTextoTarea.value.trim() !== '') {
      tareas.value[index].texto = nuevaTextoTarea.value.trim();
      guardarTareasEnLocalStorage();
      $('#editarModal').modal('hide');
    }
  };
  
  const eliminarTarea = (index) => {
    if (confirm('¿Estás seguro de eliminar esta tarea?')) {
      tareas.value.splice(index, 1);
      guardarTareasEnLocalStorage();
    }
  };
  
  // Métodos auxiliares
  const guardarTareasEnLocalStorage = () => {
    localStorage.setItem('tareas', JSON.stringify(tareas.value));
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
    text-decoration: underline;
  }
  
  </style>