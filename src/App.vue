<script>
  import { RouterView } from 'vue-router'
  import Navbar from './components/Navbar.vue'
  import Footer from './components/Footer.vue'

  export default{
    components:{
      Navbar,
      RouterView,
      Footer
    },
    data(){
      return{
        src_img: '/img/pizza96.png',
        alt_img: 'logo da pizzaria'
      }
    }
  }

</script>

<template>
  <div>
    <Navbar :src_img="src_img" :alt_img="alt_img"/>
    <RouterView />
    <Footer />
  </div>
</template>



<style>
  .main-container{
    min-height:250px;
  }
  h1{
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>