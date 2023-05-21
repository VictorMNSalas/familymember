<template>
    <v-row>

        <v-col cols="12" md="2">
            <v-select label="Service Type" v-model="service" @change="subDepartment($event)"
                :items="['Immigration', 'Criminal', 'Both', 'Referred Out', 'No Service Available', 'Wrong Number']"></v-select>
        </v-col>
        <v-col cols="12" md="2">
            <v-select label="Sub Department" v-model="Department" :items="department"></v-select>
        </v-col>
        <v-col cols="12" md="2">
            <!-- <v-select label="Name" :items="{ firstname }"></v-select> -->
            <v-select label="Name" v-model="name" @change="data($event)" :items="Firstname"></v-select>

        </v-col>
        <v-col cols="12" md="2">
            <v-text-field v-model="phone" label="Phone" :counter="12" :rules="numeroRules" required></v-text-field>
        </v-col>
        <v-col cols=" 12" md="2">
            <v-text-field v-model="deposit" label="Inicial Deposit" :rules="numeroRules" @change="depositCorrect"
                required></v-text-field>
        </v-col>
        <v-col cols="12" md="2">
            <!--  <v-text-field v-model="address" label="Address" required></v-text-field>-->
            <v-text-field v-model="fee" label="Fee" @change="feeCorrect" required :rules="numeroRules"></v-text-field>
        </v-col>
    </v-row>
</template>

<script>
//import AddRow from './AddRow.vue'
import { ref } from 'vue';
export default {
    name: 'RowForm',
    components: {
    },
    props: {
        familyData: Array,
    },
    data() {
        return {
            Firstname: '',
            name: '',
            phone: '',
            address: '',
            service: '',
            fee: '',
            deposit: '',
            Department: '',
            DepartmentC: '',
            DepartmentI: '',
            department: [],
            familyDataCompleted: [],
            outData: [],
            feeValidation: false,
            depositValidation: false

        }
    }
    ,
    computed: {
        numeroRules() {
            return [
                (value) => !!value || 'Este campo es obligatorio',
                (value) => /^[0-9]*$/.test(value) || 'Solo se permiten números'
            ]
        }
    }
    ,
    methods: {
        feeCorrect() {
            this.feeValidation = ref(true)
        },
        depositCorrect() {
            this.depositValidation = ref(true)
        },
        getData() {
            /* eslint-disable */
            let firstname = []

            console.log('props de family', this.familyData)
            this.familyData[0].forEach((item) => {
                item.forEach((data) => {
                    let name = data.Firts_Name
                    let last = data.Last_Name
                    let addres = data.Address
                    let phone = data.Phone

                    let fullName = `${name} ${last}`
                    firstname.push(fullName)
                    console.log("full name ", fullName + phone + addres)
                    this.familyDataCompleted.push({ fullName, addres, phone })
                })
            });
            this.Firstname = firstname
            console.log("na", this.Firstname)
            console.log("namesd", this.familyDataCompleted)
        },
        data(data) {
            this.familyDataCompleted.forEach((item) => {
                if (item.fullName == data) {
                    console.log(item.fullName)
                    this.phone = item.phone
                    this.address = item.addres
                }
            })
        },
        subDepartment(data) {
            console.log(data)
            if (data == "Immigration") {
                this.department = []
                this.department.push("EOIR - Undetained", "USCIS - U-Cert", "USCIS - Petitions", "SIJS", "FOIA")
            } else if (data == "Criminal") {
                this.department = []
                this.department.push("Post Conviction - Pre Filing", "Post Conviction - MTV", "Post Conviction - Petitions", "Post Conviction - Investigations", "Post Conviction - Assessments", "Misdemeanors", "Felonies")
            }
        },
    },
    mounted() {
        this.getData()
    },
    updated() {
        const fee1 = Number(this.fee);
        const depisit1 = Number(this.deposit);
        if (this.service == "Criminal") {
            this.DepartmentC = this.Department
            this.DepartmentI = ''
        }else if(this.service == "Immigration"){
            this.DepartmentI = this.Department
            this.DepartmentC = ''
        }
        if (this.feeValidation && this.depositValidation) {
            if ((this.phone && this.fee && this.deposit && this.name && this.service && this.Department) != '') {
                if (this.phone.length == 12) {
                    if (!isNaN(fee1) && !isNaN(depisit1)) {
                        let name = this.name
                        let service = this.service
                        let dep = this.Department
                        let addres = this.address
                        let phone = this.phone
                        const payload = {
                            "name": this, name,
                            "service": this.service,
                            "depC": this.DepartmentC,
                            "depI": this.DepartmentI,
                            "addres": this.address,
                            "phone": this.phone,
                            "fee": this.fee,
                            "deposit": this.deposit
                        }
                        this.outData.push(name, service, dep, addres, phone)
                        console.log(this.outData)
                        this.$emit('los_valores', payload)
                        this.feeValidation = ref(false)
                        this.depositValidation = ref(false)
                    } else {
                        alert("Has entered incorrect data in the Fee and Deposit fields. The only valid values are numbers. Please enter valid data.")
                    }
                }
            }
        }

    }

}
</script>

<style></style>