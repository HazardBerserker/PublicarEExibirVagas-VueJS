<template>
  <div class="container py-4">

    <div class="row">
      <div class="col">
        <vagas-favoritas />
      </div>
    </div>

    <div class="row mt-5">
      <div class="col">
        <pesquisar-vaga />
      </div>
    </div>

    <!-- por meio do v-slot acessamos o slot default que está no componenete filho,  
     desta forma podemos acessar os atributos deste slot, depois escolhemos uma 
     variável que receberá os valores assim como um 'props', o nome recomendado pelo 
     Vue é slotProps -->

    <!-- desta maneira podemos fazer uma exibição customizada pois recuperamos 
    o objeto vaga -->

    <!-- <br>
    <h2>Template customizado</h2>
    <br> -->

    <lista-vagas></lista-vagas>


    <!-- Multiplos slots -->
    <!-- <lista-vagas>
      <template v-slot:titulo="slotProps">
        {{ slotProps.dadosTitulo.titulo }}
        <hr>
      </template>

      <template v-slot:default="slotProps">
        {{ slotProps.vagas }}
        <hr>
      </template>

      <template v-slot:rodape="slotProps">
        {{ slotProps.dadosRodape.titulo }}
        <hr>
      </template>
    </lista-vagas> -->


    <div class="row mt-5">
      <div class="col-4">
        <indicador-layout titulo="Vagas abertas" indicador="100" bg="bg-dark" color="text-white"></indicador-layout>
      </div>

      <div class="col-4">
        <indicador-layout titulo="Profissionais cadastrados" indicador="225" bg="bg-dark"
          color="text-white"></indicador-layout>
      </div>

      <div class="col-4">
        <indicador-layout titulo="Visitantes online" :indicador="usuariosOnline" bg="bg-light"
          color="text-dark"></indicador-layout>
      </div>
    </div>

  </div>
</template>

<script>
import IndicadorLayout from '@/components/comuns/IndicadorLayout.vue'
import PesquisarVaga from '@/components/comuns/PesquisarVaga.vue'
import VagasFavoritas from '@/components/comuns/VagasFavoritas.vue'
import ListaVagas from '@/components/comuns/ListaVagas.vue'

export default {
  name: 'HomeLayout',

  components: {
    PesquisarVaga,
    IndicadorLayout,
    VagasFavoritas,
    ListaVagas
  },

  data: () => ({
    usuariosOnline: 0,
   
  }),

  methods: {
    getUsuariosOnline() {
      this.usuariosOnline = Math.floor(Math.random() * 101) //entre 0 e 100
    }
  },

  created() {
    setInterval(this.getUsuariosOnline, 1000) //a cada 1 segundo
  },


}
</script>

<style scoped></style>
