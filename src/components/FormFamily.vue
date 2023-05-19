<template>
    <v-form @submit.prevent>
        <v-container>

            <div v-for="(component, index) in componentes" :key="index">
                <RowForm :familyData="familyInfo" v-on:los_valores="getDataForm" />
            </div>
        </v-container>
        <AddRow class="d-flex justify-center" v-on:el_arreglo="onResults" />

        <v-btn class="d-flex justify-center" size="x-large" @click="sendDataForm">Submit</v-btn>

        <p>{{ formData }}</p>
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
            familyInfo: [],
            formData: []
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
        getDataForm(data) {
            console.log(958496 + data)
            this.formData.push(data)
        },
        sendDataForm() {
            this.formData.forEach((item) => {
                console.log("sad", item)
                console.log("Name", item[0])
                console.log("Service", item[1])
                console.log("Dep", item[2])
                console.log("Address", item[3])
                console.log("Phone", item[4])
                let recordData = {
                    "Client_Name": item[0],
                    "Service_Type": item[1]
                }
                console.log(recordData)
                /*ZOHO.CRM.API.insertRecord({Entity:"Deals",APIData:recordData,Trigger:["workflow"]}).then(function(data){
	console.log(data);
	});*/
            })
        }

    },
    mounted() {
        this.pushData()
    }


}
</script>

<style></style>