<template>
  <v-form @submit.prevent>
    <v-container class="spacing-playground mt-1">
      <v-alert
        v-if="CreatedRecordAlert"
        class="alert"
        text=""
        type="info"
        variant="tonal"
        >New record have been successfully created in the Cases/Retainers
        module.
      </v-alert>
      <v-alert v-if="error" class="alert" text="" type="error" variant="tonal"
        >Erroneous information has been found added in the Fee or Initial
        Deposit field.
      </v-alert>
      <v-alert
        v-if="warning"
        class="alert"
        text=""
        type="warning"
        variant="tonal"
        >There are unanswered fields, please enter the complete information to
        proceed Deposit field.
      </v-alert>

      <h3 v-if="componentes.length <= 0">
        To enter new data, please click on the (➕) button.
      </h3>
      <v-row id="row" v-for="(component, index) in componentes" :key="index">
        <v-col cols="12" md="2">
          <v-select
            label="Service Type"
            v-model="rowData[index].service"
            @change="subDepartmentOptions($event, index)"
            :items="['Immigration', 'Criminal']"
          ></v-select>
        </v-col>
        <v-col cols="12" md="2">
          <v-select
            label="Sub Department"
            v-model="rowData[index].department"
            :items="rowData[index].Departments"
            @change="subDepartment($event, index)"
          ></v-select>
        </v-col>
        <v-col cols="12" md="2">
          <v-select
            label="Name"
            v-model="rowData[index].name"
            :items="Firstname"
            @change="setData($event, index)"
          ></v-select>
        </v-col>
        <v-col cols="12" md="2">
          <v-text-field
            v-model="rowData[index].phone"
            label="Phone"
            :counter="12"
            required
            maxlength="12"
          ></v-text-field>
        </v-col>
        <v-col cols=" 12" md="2">
          <v-text-field
            v-model="rowData[index].deposit"
            label="Inicial Deposit"
            :rules="numeroRules"
            required
            prefix="$"
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="1">
          <v-text-field
            v-model="rowData[index].fee"
            label="Fee"
            required
            :rules="numeroRules"
            prefix="$"
          ></v-text-field>
        </v-col>
        <v-col cols="1" md="1" class="d-flex justify-center">
          <v-icon @click="removeItem(index)" size="large" color="red"
            >mdi-delete</v-icon
          >
        </v-col>
      </v-row>

      <div id="btns">
        <v-btn
          size="x-large"
          class="spacing-playground ma-3"
          @click="duplicarComponente"
          >+</v-btn
        >
        <v-btn
          size="x-large"
          class="spacing-playground ma-3"
          @click="dataRowGet"
          :disabled="botondisabled"
        >
          <p v-if="!loader">Submit</p>
          <v-progress-circular
            v-else
            indeterminate
            :size="25"
          ></v-progress-circular>
        </v-btn>
      </div>
    </v-container>
    <v-chip
      class="ma-10"
      closable
      color="blue"
      text-color="white"
      prepend-icon="mdi-checkbox-marked-circle"
    >
      {{ rowsCompletedCount }}/{{ rowsCount }} Created
    </v-chip>
  </v-form>
</template>

<script>
import { ref } from "vue";
export default {
  props: {
    familyData: Array,
    id_lead: String,
    Matter_Client_Number: Number,
    Matter_ID: Object,
    leadData: Array,
  },
  data() {
    return {
      componentes: [],
      rowData: [],
      familyInfo: [],
      formData: [],
      department: [],
      phones: [],
      data: "",
      CreatedRecordAlert: false,
      warning: false,
      error: false,
      loader: false,
      Firstname: "",
      botondisabled: true,
      familyDataCompleted: [],
      das: [],
      payments_plan: [],
      rowsCount: 0,
      rowsCompletedCount: 0,
    };
  },
  computed: {
    numeroRules() {
      return [
        (value) => !!value || "This field is required",
        (value) => /^[0-9]*$/.test(value) || "Only numbers allowed",
      ];
    },
  },
  methods: {
    /* eslint-disable */
    btnSumbitDisabled() {
      if (this.rowsCount > 0) {
        this.botondisabled = ref(false);
      } else {
        this.botondisabled = ref(true);
      }
    },
    duplicarComponente() {
      const nuevoComponente = "RowForm";
      this.componentes.push(nuevoComponente);
      this.rowsCount = ref(this.componentes.length);
      this.btnSumbitDisabled();
      this.rowData.push({
        service: "",
        department: "",
        Departments: [],
        name: "",
        phone: "",
        deposit: "",
        address: "",
        fee: "",
        DepartmentC: "",
        DepartmentI: "",
      });
    },
    removeItem(index) {
      this.componentes.splice(index, 1);
      this.rowData.splice(index, 1);
      this.formData.splice(index, 1);
      this.rowsCount = ref(this.componentes.length);
      this.btnSumbitDisabled();
    },
    getData() {
      if (this.familyData.length == 0) {
        this.familyData.push([]);
      }
      this.familyData[0].push(this.leadData[0]);
      let firstname = [];
      this.familyData[0].forEach((item) => {
        let name = item.Firts_Name;
        let last = item.Last_Name;
        let addres = item.Address;
        let phone = item.Phone;
        let fullName = "";
        if (last != "") {
          fullName = `${name} ${last}`;
        } else {
          fullName = `${name}`;
        }

        firstname.push(fullName);
        this.familyDataCompleted.push({ fullName, addres, phone });
      });
      this.Firstname = firstname;
    },
    setData(data, index) {
      this.familyDataCompleted.forEach((item) => {
        if (item.fullName == data) {
          this.rowData[index].phone = item.phone;
          // this.phones.push(item.phone)
          this.rowData[index].name = item.fullName;
          this.rowData[index].address = item.addres;
        }
      });
    },
    subDepartmentOptions(data, index) {
      if (data == "Immigration") {
        this.rowData[index].Departments = [];
        this.rowData[index].Departments.push(
          "EOIR - Undetained",
          "USCIS - U-Cert",
          "USCIS - Petitions",
          "SIJS",
          "FOIA"
        );
      } else if (data == "Criminal") {
        this.rowData[index].Departments = [];
        this.rowData[index].Departments.push(
          "Post Conviction - Pre Filing",
          "Post Conviction - MTV",
          "Post Conviction - Petitions",
          "Post Conviction - Investigations",
          "Post Conviction - Assessments",
          "Misdemeanors",
          "Felonies"
        );
      }
    },
    subDepartment(data, index) {
      if (this.rowData[index].service == "Criminal") {
        this.rowData[index].DepartmentC = data;
        this.rowData[index].DepartmentI = "";
      } else if (this.rowData[index].service == "Immigration") {
        this.rowData[index].DepartmentI = data;
        this.rowData[index].DepartmentC = "";
      }
    },
    dataRowGet() {
      this.rowData.forEach((element) => {
        const fee1 = Number(element.fee);
        const depisit1 = Number(element.deposit);
        if (
          (element.phone &&
            element.fee &&
            element.deposit &&
            element.name &&
            element.service &&
            element.department) != ""
        ) {
          if (element.phone.length == 12) {
            if (!isNaN(fee1) && !isNaN(depisit1)) {
              let payload = {
                name: element.name,
                service: element.service,
                depC: element.DepartmentC,
                depI: element.DepartmentI,
                addres: element.address,
                phone: element.phone,
                fee: element.fee,
                deposit: element.deposit,
              };
              this.formData.push(payload);

              let payload_payment = {
                Case_Retainer: "",
                Deposit: element.deposit,
                Amount_Paid: element.fee,
              };
              this.payments_plan.push(payload_payment);
            } else {
              this.error = ref(true);
              setTimeout(() => {
                this.error = ref(false);
              }, 3000);
            }
          }
        } else {
          this.warning = ref(true);
          setTimeout(() => {
            this.warning = ref(false);
          }, 3000);
        }
      });

      if (this.formData.length == this.rowData.length) {
        console.log(this.payments_plan);
        //this.createRecordPayments();
        this.createRecord();
      } else {
        this.formData = [];
      }
    },
    async createRecord() {
      this.loader = ref(true);
      
      this.formData.forEach((element, index) => {
        index = index + 1;
        this.data = element;
        let Matter = `${this.Matter_Client_Number} - ${index}`;
        const that = this;
        ZOHO.CRM.API.insertRecord({
          Entity: "Deals",
          APIData: {
            // Proporciona los datos del nuevo registro que deseas insertar.
            // Debes incluir los campos requeridos según la configuración de tu entidad.
            Client_Name: this.id_lead,
            Stage: "Open",
            Deal_Name: Matter,
            Name1: element.name,
            Matter: this.Matter_ID,
            Service_Type: element.service,
            Criminal_Department: element.depC,
            Immigrant_Department: element.depI,
            Adress: element.addres,
            Phone: element.phone,
            Fee: element.fee,
            Initial_Deposit: element.deposit,
          },
        })
          .then(function (response) {
            // La respuesta contiene la información del nuevo registro insertado.

            let case_id = response.data[0].details.id;
            that.das.push(case_id);
            that.rowsCompletedCount = ref(index);
            that.createRecordPayments();
          })
          .catch(function (error) {
            // En caso de error, se muestra el mensaje de error.
            console.error(error);
          });
      });
    },
    async createRecordPayments() {
      let id_case_set = 0;
      this.das.sort().forEach((element, index) => {
        console.log("kionda", element);
        this.payments_plan[index].Case_Retainer = element;
      });
      this.payments_plan.forEach((element) => {
        if (element.Case_Retainer != "") {
          id_case_set++;
        }
      });

      if (id_case_set == this.rowsCount) {
        const that = this;
        ZOHO.CRM.API.insertRecord({
          Entity: "Payments_Plans",
          APIData: {
            Name: this.Matter_Client_Number.toString(),
            Client_Matter_Number: this.id_lead,
            Cases_Retainers2: this.payments_plan,
          },
        })
          .then(function (response) {
            // La respuesta contiene la información del nuevo registro insertado.
            console.log(response);
            that.alertView();
          })
          .catch(function (error) {
            // En caso de error, se muestra el mensaje de error.
            console.error(error);
          });
      }
    },
    alertView() {
      this.loader = ref(false);
      this.CreatedRecordAlert = ref(true);
      setTimeout(() => {
        this.CreatedRecordAlert = ref(false);
        this.deleteData();
        //ZOHO.CRM.BLUEPRINT.proceed();
      }, 5000);
    },
    deleteData() {
      this.rowData = [];
      this.componentes = [];
      this.familyInfo = [];
      this.formData = [];
      this.department = [];
      this.phones = [];
      this.data = "";
      this.CreatedRecordAlert = false;
      this.warning = false;
      this.error = false;
      this.loader = false;
      this.Firstname = "";
      this.botonDeshabilitado = true;
      this.familyDataCompleted = [];
      this.das = [];
      this.rowsCompletedCount = ref(0);
    },
  },
  mounted() {
    setTimeout(() => {
      this.getData();
    }, 5000);
  },
};
</script>

<style>
#btns {
  display: flex;
  width: fit-content;
  margin: auto;
}

#row {
  display: flex;
}

#btns p {
  margin: 1rem 0;
}

h3 {
  text-align: center;
}

#delete {
  display: flex;
  justify-content: center;
  align-content: center;
}

#delete_icon {
  cursor: pointer;
}

#delete_icon:hover {
  color: red;
}
</style>
