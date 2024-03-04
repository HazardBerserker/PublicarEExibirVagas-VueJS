<template>
    <div :class="getEstilo().colLayout">
        <div :class="getEstilo().divLayout">
            <p>{{ titulo }}</p>
            <h2>{{ indicador }}</h2>
        </div>
    </div>
</template>

<script>
export default {
    name: "IndicadorLayout",

    data() {
        return {
            deviceWidth: 0,
        };
    },

    props: ['titulo', 'indicador', 'bg', 'color'],

    methods: {
        atualizarLargura() {
            this.deviceWidth = window.innerWidth;
        },

        getEstilo() {
            if (this.deviceWidth > 575) {
                return {
                    divLayout: `h-100 p-3 rounded-1 border d-flex flex-column-reverse justify-content-between ${this.bg} ${this.color}`, 
                    colLayout: `col-4`
                };
            
            } else {
                return {
                    divLayout: `h-100 p-3 rounded-1 border d-flex flex-column-reverse justify-content-between ${this.bg} ${this.color}`,
                    colLayout: `col-12 mt-1`
                }
            }
        },

    },

    mounted() {
        this.atualizarLargura();
        window.addEventListener('resize', this.atualizarLargura);
    },

    beforeUnmount() {
        window.removeEventListener('resize', this.atualizarLargura);
    }
};
</script>
