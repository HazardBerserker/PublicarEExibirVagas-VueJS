<template>
  <div>
    <topo-padrao @navegar="componente = $event" />

    <alerta-box v-if="exibirAlerta" :tipo="alerta.tipo">
      <!-- v-slot permite escrever dentro das tags de componentes passando valores -->
      <template v-slot:titulo>
        <h5>{{ alerta.titulo }}</h5>
      </template>

      <p>{{ alerta.descricao }}</p>
    </alerta-box>

    <conteudo-layout v-if="visibilidade" :conteudo="componente"></conteudo-layout>
  </div>
</template>

<script>
import AlertaBox from '@/components/comuns/AlertaBox.vue'
import ConteudoLayout from '@/components/layouts/ConteudoLayout.vue'
import TopoPadrao from '@/components/layouts/TopoPadrao.vue'

export default {
  name: 'App',

  data: () => ({
    visibilidade: true,
    componente: 'HomeLayout',
    exibirAlerta: false,
    alerta: { titulo: '', descricao: '', tipo: '', time: ''}
  }),

  components: {
    AlertaBox,
    ConteudoLayout,
    TopoPadrao
  },

  mounted() {
    //this.emitter.on fica escutando eventos de 'alerta'
    //se escutar algum então exibirAlerta passa a ser true
    //após um certo tempo ele se torna falso e encerra sua exibição
    this.emitter.on('alerta', (a) => {

      this.alerta = a
      this.exibirAlerta = true


      setTimeout(() => this.exibirAlerta = false, this.alerta.time)
      console.log('Apresentar a mensagem de alerta customizada')
    })
  },
}
</script>