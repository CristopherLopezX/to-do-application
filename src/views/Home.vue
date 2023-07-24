<template>
  <div class="home">
    <Navbar/>
    <ListaToDo/>
  </div>
</template>

<script>
import ListaToDo from '../components/ListaToDo.vue';
import Navbar from '../components/Navbar.vue';
// @ is an alias to /src
// <PruebaToDo/>

export default {
  name: 'Home',
  components: {
    Navbar,
    ListaToDo
  }
}
</script>
