<template>
    <v-form @submit.prevent>
        <v-container>

            <div v-for="(component, index) in componentes" :key="index">
                <RowForm :familyData="familyInfo" />
            </div>
        </v-container>
        <AddRow class="d-flex justify-center" v-on:el_arreglo="onResults" />

        <v-btn class="d-flex justify-center" size="x-large">Submit</v-btn>


    </v-form>
</template>

<script>
import AddRow from './AddRow.vue'
import RowForm from './RowForm.vue'



export default {
    components: {
        RowForm,
        AddRow
    },
    props: {
        familyData: Array,
    }
    ,
    data() {
        return {
            componentes: [],
            familyInfo: []
        }
    },
    methods: {
        /* eslint-disable */

        pushData() {
            this.familyInfo.push(this.familyData)
            console.log('family', this.familyInfo)
        },
        onResults(data) {
            this.componentes.push(data)
            console.log(componentes)
        },
        getDataForm(){
            console.log(958496)
        }

    },
    mounted() {
        this.pushData()


    }


}
</script>

<style></style>