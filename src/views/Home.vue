<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Salaris msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Salaris from '@/components/Salaris.vue'

export default {
  name: 'Home',
  components: {
    Salaris
  }
}
</script>
