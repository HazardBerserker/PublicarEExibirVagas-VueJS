<template>
  <div class="container mt-4">

    <!-- Paginação -->
    <nav aria-label="Page navigation" class="mb-6">
      <ul class="pagination justify-content-center">
        <li class="page-item" :class="{ 'disabled': currentPage === 1 }">
          <button class="page-link" @click="previousPage" :disabled="currentPage === 1">Anterior</button>
        </li>
        <li class="page-item" v-for="page in totalPages" :key="page" :class="{ 'active': currentPage === page }">
          <button class="page-link" @click="goToPage(page)">{{ page }}</button>
        </li>
        <li class="page-item" :class="{ 'disabled': currentPage === totalPages }">
          <button class="page-link" @click="nextPage" :disabled="currentPage === totalPages">Próxima</button>
        </li>
      </ul>
    </nav>

    <!-- Loop para replicar todos os elementos da lista 'vagas' -->
    <div class="row">
        <div v-for="(vaga, index) in vagasToDisplay" :key="index" class="mt-4">
          <!-- Adicionando classe "w-100" para garantir largura máxima -->
              <vaga-layout v-bind="vaga" />
        </div>
    </div>


    <!-- Slot para o rodapé -->
    <!-- <slot name="rodapé" :dadosRodape="{titulo: 'Rodapé lista', paginacao: {nroPaginas: 10, paginaAtual: 5}}">
      <p>Rodapé da lista de vagas</p>
    </slot> -->

  </div>
</template>

<script>
import VagaLayout from '@/components/comuns/VagaLayout.vue'

export default {
  name: 'ListaVagas',
  components: {
    VagaLayout
  },
  data() {
    return {
      vagas: [],
      itemsPerPage: 6,
      currentPage: 1
    }
  },
  computed: {
    totalPages() {
      return Math.ceil(this.vagas.length / this.itemsPerPage);
    },
    startIndex() {
      return (this.currentPage - 1) * this.itemsPerPage;
    },
    endIndex() {
      return this.startIndex + this.itemsPerPage;
    },
    vagasToDisplay() {
      return this.vagas.slice(this.startIndex, this.endIndex);
    }
  },
  methods: {
    // Ir para a página anterior
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    // Ir para a próxima página
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    // Ir para uma página específica
    goToPage(page) {
      this.currentPage = page;
    }
  },
  activated() {
    // Recuperando os itens da chave 'vagas' dentro do localStorage
    if (this.vagas === '' || this.vagas.length === 0) {
      this.vagas = JSON.parse(localStorage.getItem('vagas'))
    } else {
      // O parâmetro (vaga) é o objeto.titulo da barra de pesquisa em pesquisar vaga
      this.emitter.on('filtrarVagas', (vaga) => {
        const vagas = JSON.parse(localStorage.getItem('vagas'))
        this.vagas = vagas.filter((reg) => reg.titulo.toLowerCase().includes(vaga.titulo.toLowerCase()))
        this.currentPage = 1; // Reiniciar a página ao filtrar
      })
    }
  },
  mounted() {
    // Está escutando eventos com o nome de 'filtrarVagas'
    this.emitter.on('filtrarVagas', (vaga) => {
      const vagas = JSON.parse(localStorage.getItem('vagas'))
      //filter é um método nativo do JavaScript e atua sobre arrays
      //ele irá retornar determinado indice em função de uma condição true ou false
      //ele cria um novo array com todos os elementos que passaram no teste
      //implementado na função.
      //reg é um registro do array, o filter percorrerá por todos os titulos de cada
      //reg em letra minuscula, se o registro incluir um valor igual ao parâmetro vaga
      //que está sendo passado no componente PesquisarVaga, ele será incluido no 
      //novo array
      this.vagas = vagas.filter((reg) => reg.titulo.toLowerCase().includes(vaga.titulo.toLowerCase()))
      this.currentPage = 1; // Reiniciar a página ao filtrar
    })
  },
}
</script>

<style scoped>
/* Adicione estilos personalizados aqui, se necessário */
</style>
