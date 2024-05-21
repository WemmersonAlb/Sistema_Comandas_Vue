<script>
import Banner from '../components/Banner.vue'
import PizzaForm from '../components/PizzaForm.vue'

export default{
  components:{
    Banner,
    PizzaForm
  }
}
</script>

<template>
  <div class="main-container">
    <Banner />
    <h1>Monte a sua pizza</h1>
    <PizzaForm class="formulario"/>
  </div>
</template>
<style>
h1{
  font-weight: 800;
  margin-top: 30px;
}

/* Mobile */
@media(max-width:600px){
  h1{
    font-size: 30px!important;
  }
  .formulario{
    margin: 0 30px;
  }
}
</style>