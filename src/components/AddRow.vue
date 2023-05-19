<template>
    <v-col>
        <v-btn  size="x-large" @click="duplicarComponente">+</v-btn >
    </v-col>
</template>

<script>


export default {

    data() {
        return {
            componentes: []
        }
    },
    methods: {
        duplicarComponente() {
            const nuevoComponente = 'RowForm'// Aquí debes crear una copia del componente a duplicar
            this.componentes.push(nuevoComponente);
            console.log(`componentes ${this.componentes}`)
            this.$emit('el_arreglo', this.componentes)
        }

    },
}
</script>

<style>

</style>