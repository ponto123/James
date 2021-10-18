<template>
  <div class="about">
    <h1>Ola Gabriel </h1>
    <button class="button is-primary" @click="bto">Botão do TESTE</button>
  </div>
</template>


<script>
import axios from 'axios'
export default {
  created(){
    //console.log("OLA");
  },
  methods:{
    bto(){
      axios.get("http://localhost:8080/teste").then(res =>{ 
        console.log(res); 
      }).catch(err =>{
          console.log(err);
      })
    }
  },
  data(){
  },
}
</script>