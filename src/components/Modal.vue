<template>
    <div>
      <div :class="{modal: true, 'is-active': showModalFlagTeste}">
        <div class="modal-background"></div>
        <div class="modal-content">
            <div class="card">
                <header class="card-header">
                    <p class="card-header-title">
                        Você quer realmente deletar este Equipamento?
                    </p>
                </header>
                <div class="card-content">
                    <div class="content">
                        Equipametos deletados não há nenhuma possibilidade de recuperar
                    </div>
                </div>
                <footer class="card-footer">
                    <a class="card-footer-item" href="#"  @click="cancelModalTeste">Cancelar</a>
                    <a class="card-footer-item" href="#">Sim, eu quero</a>
                </footer>
            </div>
        </div>
        <button class="modal-close is-large" aria-label="close" @click="cancelModalTeste"></button>
      </div>
      <button class="button is-danger is-rounded"  @click="showModalTESTE">TESTE</button>
    </div>
</template>

<script>
export default {
data(){
  return{
    showModalFlagTeste: false
  }
},
methods:{
  cancelModalTeste(){
    this.showModalFlagTeste = false;
  },
  showModalTESTE(){
      this.showModalFlagTeste = true;
    }
}
}
</script>

<style scoped>

</style>
