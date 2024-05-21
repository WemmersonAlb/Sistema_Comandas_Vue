<template>
  <div class="main-container">
    <h1>Pedidos</h1>
    <Dashboard />
  </div>
</template>
<script>
import Dashboard from '../components/Dashboard.vue'

export default {
  components:{
    Dashboard
  }
}
</script>
<style scoped>
@media(prefers-color-scheme: dark){
  h1{
    color: rgb(233, 230, 230);
  }
}
</style>