<template>
  <div>
    <keep-alive>
      <component :is="conteudo" />
    </keep-alive>
  </div>
</template>

<script>

import HomeLayout from '@/components/views/HomeLayout.vue'
import PublicarVaga from '@/components/views/PublicarVaga.vue'

export default {
  name: 'ConteudoLayout',

  props: {
    conteudo: {
      type: String,
      required: true
    }
  },

  components: {
    HomeLayout,
    PublicarVaga
  }
}
</script>

<style module>
.teste {
  background-color: red
}
#teste-2 {
  background-color: yellow
}
</style>
