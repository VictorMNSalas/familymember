<template>
    <v-form @submit.prevent>
        <v-container>
            <v-alert v-if="alert" text="" type="info" variant="tonal">New record have been successfully created in the
                Cases/Retainers module.</v-alert>
            <h3 v-if="componentes.length <= 0">To enter new data, please click on the (➕) button.</h3>

            <div v-for="(component, index) in componentes" :key="index">
                <RowForm :familyData="familyInfo" v-on:los_valores="getDataForm" />
            </div>
        </v-container>
        <div id="btns">
            <AddRow v-on:el_arreglo="onResults" />

            <v-btn size="x-large" class="spacing-playground ma-3" @click="createRecord()">
                <p v-if="!alert">Submit</p>
                <v-progress-circular v-else indeterminate  :size="25"></v-progress-circular>
            </v-btn>

        </div>

        <p>{{ formData }}</p>
        <p>{{ data }}</p>
    </v-form>
</template>

<script>
import { ref } from 'vue'
import AddRow from './AddRow.vue'
import RowForm from './RowForm.vue'
//import { ref } from 'vue';


export default {
    components: {
        RowForm,
        AddRow
    },
    props: {
        familyData: Array,
        id_lead: Number,
        Matter_Client_Number: Number
    }
    ,
    data() {
        return {
            componentes: [],
            familyInfo: [],
            formData: [],
            data: '',
            alert: false,
            
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

        async createRecord() {
            this.alert = ref(true)
            /*
            this.formData.forEach((element, index) => {
                index = index + 1
                console.log(element.name, index)
                this.data = element
                let Matter = `${this.Matter_Client_Number} - ${index}`
                console.log("subit", Matter)
                const that = this
                ZOHO.CRM.API.insertRecord({
                    Entity: "Deals",
                    APIData: {
                        // Proporciona los datos del nuevo registro que deseas insertar.
                        // Debes incluir los campos requeridos según la configuración de tu entidad.
                        Client_Name: this.id_lead,
                        Stage: "Open",
                        Deal_Name: Matter,
                        Name1: element.name,
                        Service_Type: element.service,
                        Criminal_Department: element.depC,
                        Immigrant_Department: element.depI,
                        Adress: element.addres,
                        Phone: element.phone,
                        Fee: element.fee,
                        Initial_Deposit: element.deposit
                    }
                }).then(function (response) {
                    // La respuesta contiene la información del nuevo registro insertado.
                    console.log(response)
                    that.alertView(response)

                }).catch(function (error) {
                    // En caso de error, se muestra el mensaje de error.
                    console.error(error);
                });
            });
            */

        },
        alertView(data) {
            this.alert = ref(true)
            console.log("Entro", data)
            setTimeout(() => {
                console.log('Han pasado 5 segundos');
                this.alert = ref(false)
                this.deleteData()
            }, 3000);
        },
        deleteData() {
            this.componentes = []
            this.familyInfo = []
            this.formData = []
            this.data = ''
        }

    },
    mounted() {
        this.pushData()
    }


}
</script>

<style>
#btns {
    display: flex;
    width: fit-content;
    margin: auto;
    
}
#btns p{
    margin: 1rem 0;
}
h3 {
    text-align: center;
}
</style>