<template>
    <div>      
      <div>
        <button class="button is-success is-outlined">Computador</button>
        <button class="button is-success is-outlined">Inventário</button>
      </div>
      <br>
      <div>
    </div>
      <hr>
      <input class="input" type="text" name="" id="" placeholder="Digita aqui o que vc procura" v-model="busca">
      <button class="button" @click="showModal">+</button>

      <!-- MODAL DE CADASTRO PRODUTO-->
      <div class="modal" :class="{'is-active': showModalFlagAdd}">
        <div class="modal-background"></div>
        <div class="modal-card">
          <div v-if="error != undefined">
              <div class="notification is-danger">
                <button class="delete"></button>
                <p>{{error}}</p>
              </div>
            </div>
          <header class="modal-card-head">
            <p class="modal-card-title">Cadastro de Equipamento</p>
            <button class="delete" aria-label="close" @click="cancelModal">></button>
          </header>
          <section class="modal-card-body">
            <label>Nome:</label>
            <input class="input is-rounded" type="text" v-model="nameEquipamento" placeholder="Digite nome do equipamento"><br>
            <label>Quantidade</label>
            <input class="input is-rounded" type="number" v-model="qtdEquipamento" placeholder="Escolha ou Digite a quantidade">            
          </section>
          <footer class="modal-card-foot">
            <button class="button is-success" @click="okModal">Cadastrar</button>
            <button class="button" @click="cancelModal">Cancel</button>
          </footer>
        </div>
      </div>
       <!-- FIM MODAL DE CADASTRO PRODUTO-->

      <!-- MODAL DE DELETAR--> 
      <div :class="{modal: true, 'is-active': showModalFlagDell}">
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
                    <a class="card-footer-item" href="#"  @click="cancelModalDell">Cancelar</a>
                    <a class="card-footer-item" href="#" @click="ConfDeleteEquipe">Sim, eu quero</a>
                </footer>
            </div>
        </div>
        <button class="modal-close is-large" aria-label="close" @click="cancelModalDell"></button>
      </div>
      <!-- FIM MODAL DE DELETAR-->  
       <Editar @meDelete="deletarUsuario"/>
        <table class="table is-hoverable">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">Produto</th>
              <th scope="col">Quantidade</th>
              <th scope="col"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="result, index in this.resultadoBusca" :key="result.id">
              <td>{{index}}</td>
              <td class="suporte">{{result.descricao}}</td>
              <td class="suporte">{{result.qtd}}</td>
              <td><button class="button is-warning is-rounded">EDITAR</button></td>
              <td><button class="button is-danger is-rounded" @click="showModalDell(result.id)">DELEAR</button></td>
            </tr>
          </tbody>
        </table>
       
    </div>
</template>

<script>
import axios from 'axios';
import Editar from '../../components/Editar.vue'
export default {
  created(){
    axios.get("http://localhost:8080/inventario").then(res=>{
      this.results = res.data;
    }).catch(err =>{
            console.log(err);
      })
  },
  components:{
    Editar
  },
  data(){
    return{
      showModalFlagAdd: false,
      okPressed: false,
      message: "Press 'Ok' or 'Cancel'.",
      results:[],
      busca: '',
      nameEquipamento: '',
      qtdEquipamento: '',
      error: undefined,
      deleteEquipe: -1,
      showModalFlagDell: false
    }
    
  },
  methods: {
    deletarUsuario(){
      console.log("PQP funciona");
    },
    showModalDell(id){
      this.deleteEquipe = id;
      this.showModalFlagDell = true;
    },
    cancelModalDell(){
      this.deleteEquipe = -1;
      this.showModalFlagDell = false;
    },
    ConfDeleteEquipe(){
      axios.delete("http://localhost:8080/inventario/"+ this.deleteEquipe).then(res=>{
          console.log(res);
          axios.get("http://localhost:8080/inventario").then(res=>{
                this.results = res.data;
          }).catch(err =>{
                  console.log(err);
            });
          this.showModalFlagDell = false;
          //this.users = this.users.filter(u => u.id != this.deleteUserId);
      }).catch(err=>{
          console.log(err);
          this.showModalFlagDell = false;
      });
    },
    showModal() {
      this.okPressed = false;
      this.showModalFlagAdd = true;
    },
    okModal() {
      axios.post("http://localhost:8080/inventario",{
              equipamento: this.nameEquipamento,
              qtd: this.qtdEquipamento
          }).then(res =>{
              console.log(res);
              //this.ordens = res.data;
              axios.get("http://localhost:8080/inventario").then(res=>{
                this.results = res.data;
              }).catch(err =>{
                      console.log(err);
                });
              this.cancelModal();
          }).catch(err=>{
              var msgErro = err.response.data.err;
              this.error = msgErro;
          });
    },
    cancelModal() {
      this.nameEquipamento = '';
      this.qtdEquipamento = '';
      this.error = undefined;
      this.okPressed = false;
      this.showModalFlagAdd = false;
    }
  },
  computed:{
    resultadoBusca(){
      var filtro = [];

      filtro = this.results.filter((result) =>{
        return(
          result.descricao.toLowerCase().indexOf(this.busca.toLowerCase()) > -1
        );
      })
      return filtro;
    }
  }
}
</script>


<style scoped>
.suporte{
  padding-top: 15px;
}
</style>