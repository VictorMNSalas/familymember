<template>
    <v-form @submit.prevent>

        <v-container class="spacing-playground mt-5">
            <v-alert v-if="alert" class="alert" text="" type="info" variant="tonal">New record
                have been successfully created in the
                Cases/Retainers module.
            </v-alert>
            <v-alert v-if="error" class="alert" text="" type="error" variant="tonal">Erroneous information has been found
                added in the
                Fee or Initial Deposit field.
            </v-alert>
            <v-alert v-if="warning" class="alert" text="" type="warning" variant="tonal">There are unanswered fields, please
                enter the
                complete information to proceed Deposit field.
            </v-alert>
            <v-row>
                <v-col cols="12" md="2">
                    <v-text-field v-model="service" label="Service Type" required></v-text-field>
                </v-col>
                <v-col cols="12" md="2">
                    <v-select label="Sub Department" v-model="Department" :items="department"></v-select>
                </v-col>
                <v-col cols="12" md="2">
                    <v-text-field v-model="name" label="Name" required></v-text-field>

                </v-col>
                <v-col cols="12" md="2">
                    <v-text-field v-model="phone" label="Phone" required></v-text-field>
                </v-col>
                <v-col cols=" 12" md="2">
                    <v-text-field v-model="deposit" label="Inicial Deposit" :rules="numeroRules" @change="depositCorrect"
                        required></v-text-field>
                </v-col>

                <v-col cols="12" md="2">
                    <v-text-field v-model="fee" label="Fee" @change="feeCorrect" required
                        :rules="numeroRules"></v-text-field>
                </v-col>
            </v-row>

        </v-container>
        <div id="btns">
            <v-btn size="x-large" class="spacing-playground ma-3" @click="createRecord()">
                <p v-if="!loader">Submit</p>
                <v-progress-circular v-else indeterminate :size="25"></v-progress-circular>
            </v-btn>
        </div>
    </v-form>
</template>

<script>
import { ref } from 'vue'
export default {
    data() {
        return {
            name: '',
            phone: '',
            address: '',
            service: '',
            fee: '',
            deposit: '',
            department: [],
            Department: '',
            depC: '',
            depI: '',
            alert: false,
            warning: false,
            error: false,
            loader: false
        }
    },
    props: {
        leadData: Array,
        id_lead: Number,
        Matter_Client_Number: Number,
        Matter_ID: Number
    },
    computed: {
        numeroRules() {
            return [
                (value) => !!value || 'This field is required',
                (value) => /^[0-9]*$/.test(value) || 'Only numbers allowed'
            ]
        }
    }
    ,
    methods: {

        async assignInfo() {
            //console.log(this.leadData[0])
            this.name = this.leadData[0].name
            this.phone = this.leadData[0].mobile
            this.service = this.leadData[0].service
            this.address = this.leadData[0].address
            this.subDepartment()
        },
        subDepartment() {
            // console.log(data)
            if (this.service == "Immigration") {
                this.department = []
                this.department.push("EOIR - Undetained", "USCIS - U-Cert", "USCIS - Petitions", "SIJS", "FOIA")
            } else if (this.service == "Criminal") {
                this.department = []
                this.department.push("Post Conviction - Pre Filing", "Post Conviction - MTV", "Post Conviction - Petitions", "Post Conviction - Investigations", "Post Conviction - Assessments", "Misdemeanors", "Felonies")
            } else {
                this.department = []
            }
        },
        /*  deleteRow(index){
             // console.log("delete: ", index)
             // console.log(this.componentes[0]);
           //   this.componentes[0].splice(index, 1);
            //  console.log(this.componentes[0]);
          },*/
        async createRecord() {
            /* eslint-disable */

            const fee1 = Number(this.fee);
            const depisit1 = Number(this.deposit);
            if (this.service == "Criminal") {
                this.depC = this.Department
                this.depI = ''
            } else if (this.service == "Immigration") {
                this.depI = this.Department
                this.depC = ''
            }
            if ((this.phone && this.fee && this.deposit && this.name && this.service && this.Department) != '') {
                if (!isNaN(fee1) && !isNaN(depisit1)) {
                    this.loader = ref(true)
                    let Matter = `${this.Matter_Client_Number} - 1`
                    const that = this
                    ZOHO.CRM.API.insertRecord({
                        Entity: "Deals",
                        APIData: {
                            // Proporciona los datos del nuevo registro que deseas insertar.
                            // Debes incluir los campos requeridos según la configuración de tu entidad.
                            Client_Name: this.id_lead,
                            Stage: "Open",
                            Deal_Name: Matter,
                            Name1: this.name,
                            Matter: this.Matter_ID,
                            Service_Type: this.service,
                            Criminal_Department: this.depC,
                            Immigrant_Department: this.depI,
                            Adress: this.address,
                            Phone: this.phone,
                            Fee: this.fee,
                            Initial_Deposit: this.deposit
                        }
                    }).then(function (response) {
                        // La respuesta contiene la información del nuevo registro insertado.
                        console.log(response)
                        that.alertView(response)

                    }).catch(function (error) {
                        // En caso de error, se muestra el mensaje de error.
                        console.error(error);
                    });

                } else {
                    this.error = ref(true)
                    //  console.log("Entro", data)
                    setTimeout(() => {
                        this.error = ref(false)
                    }, 3000);
                }
            } else {
                this.warning = ref(true)
                //  console.log("Entro", data)
                setTimeout(() => {
                    this.warning = ref(false)
                }, 3000);
            }
        },
        alertView() {
            this.alert = ref(true)
            setTimeout(() => {
                this.alert = ref(false)
                this.loader = ref(false)
            }, 3000);
        },
    },
    mounted() {
        this.assignInfo()
    }
}
</script>

<style>
.spacing-playground {
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

#loader {
    margin: auto;
}
</style>