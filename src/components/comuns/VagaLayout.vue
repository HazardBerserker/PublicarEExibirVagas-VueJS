<template>
    <div class="card">
        <div class="card-header bg-dark text-white">
            <div class="row">
                <div class="col d-flex justify-content-between">
                    <div>
                        {{ titulo }}
                    </div>
                    <div>
                        <div class="form-check form-switch">
                            <input class="form-check-input" type="checkbox" v-model="favoritada">
                            <label class="form-check-label">Favoritar</label>
                        </div>
                    </div>
                </div>
            </div>

        </div>
        <div class="card-body">
            <p>{{ descricao }}</p>
        </div>
        <div class="card-footer">
            <small class="text-muted">Salário: R$ {{ salario }} | Modalidade: {{ getModalidade }} | Tipo: {{ getTipo }} |
                Publicação: {{ getPublicacao }}</small>
        </div>
    </div>
</template>

<script>
export default {
    name: 'VagaLayout',

    data: () => ({
        favoritada: false
    }),

    watch: {
        favoritada(valorNovo) {
            if (valorNovo) {
                this.emitter.emit('favoritarVaga', this.titulo)
            } else {
                this.emitter.emit('desfavoritarVaga', this.titulo)
            }
        }
    },

    //props: ['tituloVagaTeste', 'descricaoVaga', 'salario', 'modalidade', 'tipo', 'publicacao'],
    //o props recebe os valores do elemento pai que está chamando-o
    //neste caso os valores estão em uma lista dentro do elemento pai
    //o props recebe os valores de maneira automatica e preenche o layout
    //é necessario que cada propriedade do props tenha o mesmo nome
    //das propriedades do objeto dentro da lista do elemento pai
    props: {
        titulo: {
            type: String,
            required: true,
            validator(p) {
                //console.log('Prop: ', p, )
                if (p.length < 6) return false //se estiver inválido
                return true //se estiver válido
            }
        },
        descricao: {
            type: String,
            //default: 'O contratante não adicinou uma descrição para essa vaga'
            default() {
                return '*'.repeat(20)
            }
        },
        salario: {
            type: [Number, String],
            required: true
        },
        modalidade: {
            type: String,
            required: true
        },
        tipo: {
            type: String,
            required: true
        },
        publicacao: {
            type: String,
            required: true
        }
    },

    computed: {
        getModalidade() {
            switch (this.modalidade) {
                case '1': return 'Home Office'
                case '2': return 'Presencial'
            }
            return ''
        },
        getTipo() {
            switch (this.tipo) {
                case '1': return 'CLT'
                case '2': return 'PJ'
            }
            return ''
        },
        getPublicacao() {
            //criando um objeto de data com base nos valores de data
            //recebidos pelo props
            let dataPublicacao = new Date(this.publicacao)

            //return dataPublicacao.toLocaleString('pt-BR')
            return dataPublicacao.toLocaleDateString('pt-BR')
        }
    },
}
</script>