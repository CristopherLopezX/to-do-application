<template>
    <div id="myModalEli" class="modalEli" v-show="showModal">
      <div class="modal-content">
        <p>eliminar</p>
        <div>
          <button class="btn-si" @click="eliminarTareaConfirmado">SI</button>
          <button class="btn-no" @click="showModal = false">NO</button>
        </div>
      </div>
    </div>
</template>


<script setup>


</script>

<style scoped>
.modal {
    display: none;
    /* Hidden by default */
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
