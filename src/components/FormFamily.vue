<template>
    <v-form @submit.prevent>
        <v-container>

            <h3 v-if="componentes.length <= 0">To enter new data, please click on the (➕) button.</h3>

            <div v-for="(component, index) in componentes" :key="index">
                <RowForm :familyData="familyInfo" v-on:los_valores="getDataForm" />
            </div>
        </v-container>
        <div id="btns">
            <AddRow v-on:el_arreglo="onResults" />

            <v-btn size="x-large" @click="createRecord()">Submit</v-btn>

        </div>

        <p>{{ formData }}</p>
        <p>{{ data }}</p>
    </v-form>
</template>

<script>
import AddRow from './AddRow.vue'
import RowForm from './RowForm.vue'
import { ref } from 'vue';


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
            data: ''
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
        /*sendDataForm() {
            const num = 1
            this.formData.forEach((item, index) => {
                index = index + num
                console.log("sad", item)
                console.log("Name", item[0])
                console.log("Service", item[1])
                console.log("Dep", item[2])
                console.log("Address", item[3])
                console.log("Phone", item[4])
                let recordData = {
                    "Case_Retainer_Name": this.id_lead,
                    "Service_Type": item[1],
                    "Client_Name": this.id_lead,
                    "Name1": item[0],
                    "Phone": item[4],
                    "Adress": item[3],
                    "Sub_departament_Criminal": '',
                    "Sub_departament_Immigration": item[2],
                    "Client_Name": item[1],
                }
                console.log(index, recordData)
                const newRecord = ZOHO.CRM.API.insertRecord({ Entity: "Deals", APIData: recordData})
            })
        }*/
        async createRecord() {
            let records_created = ref(0)
            this.formData.forEach((element, index) => {
                index = index + 1
                console.log(element.name, index)
                this.data = element
                let Matter = `${this.Matter_Client_Number} - ${index}`
                console.log("subit", Matter)
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
                    console.log(response);
                    records_created = records_created + 1
                }).catch(function (error) {
                    // En caso de error, se muestra el mensaje de error.
                    console.error(error);
                });
            });
            console.log(records_created)
            // alert("New records have been successfully created in the Cases / Retainers module. ✔️")
            /*
              ZOHO.CRM.API.insertRecord({
                    Entity: "Deals",
                    APIData: {
                        // Proporciona los datos del nuevo registro que deseas insertar.
                        // Debes incluir los campos requeridos según la configuración de tu entidad.
                        Case_Retainer_Name: Matter,
                        Client_Name: this.id_lead,
                        Stage: "Open",
                        Deal_Name: "Deals",
                        Name1: element.name,
                        Service_Type: element.service,
                        Sub_departament_Criminal: element.dep,
                        //Sub_departament_Immigration: element.dep,
                        Adress: element.addres,
                        Phone: element.phone,
                        Fee: element.fee,
                        Initial_Deposit: element.deposit
                    }
                }).then(function (response) {
                // La respuesta contiene la información del nuevo registro insertado.
                console.log(response);
            }).catch(function (error) {
                // En caso de error, se muestra el mensaje de error.
                console.error(error);
            });
            */
        }

    },
    mounted() {
        this.pushData()
    }


}
</script>

<style>
#btns {
    display: flexbox;
    width: fit-content;
    margin: auto;
}

h3 {
    text-align: center;
}
</style>