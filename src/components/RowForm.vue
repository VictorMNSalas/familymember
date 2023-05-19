<template>
    <v-row>
        <v-col cols="12" md="3">
            <!-- <v-select label="Name" :items="{ firstname }"></v-select> -->
            <v-select label="Name" v-model="name" @change="data($event)" :items="Firstname"></v-select>

        </v-col>
        <v-col cols="12" md="2">
            <v-text-field v-model="phone" label="Phone" required></v-text-field>
        </v-col>
        <v-col cols="12" md="2">
            <v-text-field v-model="address" label="Address" required></v-text-field>
        </v-col>
        <v-col cols="12" md="2">
            <v-select label="Service Type" v-model="service" @change="subDepartment($event)"
                :items="['Immigration', 'Criminal', 'Both', 'Referred Out', 'No Service Available', 'Wrong Number']"></v-select>
        </v-col>
        <v-col cols="12" md="2">
            <v-select label="Sub Department" v-model="Department" :items="department"></v-select>
        </v-col>

    </v-row>
</template>

<script>
//import AddRow from './AddRow.vue'
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
            Department: '',
            department: [],
            familyDataCompleted: [],
            outData: []

        }
    }
    ,
    methods: {
        /*
        duplicarComponente() {
            const nuevoComponente = 'RowForm'// Aquí debes crear una copia del componente a duplicar
            this.componentes.push(nuevoComponente);
            console.log(`Componentes ${this.componentes}`)
            this.$emit('el_arreglo', this.componentes)
        },
       */
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
        if ((this.phone && this.address && this.name && this.service && this.Department) != '') {
            let name = this.name
            let service = this.service
            let dep = this.Department
            let addres = this.address
            let phone = this.phone
            this.outData.push(name, service, dep, addres, phone)
            console.log(this.outData)
            this.$emit('los_valores', this.outData)
        }

    }

}
</script>

<style></style>