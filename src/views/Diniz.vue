<template>
  <div class="about">
      <div>
          <button class="button is-success is-outlined" @click="bto">Botão do TESTE</button>
          <button class="button is-success is-outlined" @click="inventario">Inventário</button>
      </div>
  </div>
</template>


<script>
import axios from 'axios'
export default {
  created(){
  },
  methods:{
    bto(){
      axios.get("http://localhost:8080/teste").then(res =>{ 
        console.log(res); 
      }).catch(err =>{
          console.log(err);
      })
    },
    inventario(){
      axios.get("http://localhost:8080/inventario").then(res =>{
        console.log(res);
        this.$router.push({name: 'Inventario'}); 
      }).catch(err=>{
          var msgErro = err.response.data.err;
          this.error = msgErro;
        })
    }
  }
}
</script>

<style>
.button{
    margin-right: 3px;
}
</style>