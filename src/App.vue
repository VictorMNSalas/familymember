<template>
  <v-app>
    <v-main>
      <HeaderApp />
      <FormDataMembers :leadData="leadData" :Matter_ID="Matter_ID" :Matter_Client_Number="Matter_Client_Number"
        :id_lead="id_lead" :familyData="familyMembers" />
      <!--
<FormFamilyVue :Matter_ID="Matter_ID" :Matter_Client_Number="Matter_Client_Number" :id_lead="id_lead"
        :familyData="familyMembers" />

      -->
    </v-main>
  </v-app>
</template>

<script>

import HeaderApp from './components/HeaderApp.vue';
//import ACaseOnly from './components/ACaseOnly.vue';
import FormDataMembers from './components/FormDataMembers.vue';

export default {
  name: 'App',

  components: {
    HeaderApp,
    //  ACaseOnly,
    FormDataMembers
  },

  data: () => ({
    familyMembers: [],
    id_lead: '',
    Matter_Client_Number: '',
    Matter_ID: '',
    leadData: []
    //
  }),
  mounted() {
    /* eslint-disable */
    /*ZOHO.embeddedApp.on("PageLoad", function (info) {
      ZOHO.CRM.API.getRecord({ Entity: "Contacts", RecordID: "5029988000048681622" })
        .then(function (info) {
          console.log("Record", info)
        })
    })
    ZOHO.embeddedApp.init();
    */
    //const that = this
    ZOHO.embeddedApp.on("PageLoad", async (data) => {

      const crmId = data.EntityId
      const crmRecord = data.Entity
      this.id_lead = crmId
      //console.log('id', crmId)
      //console.log('modulo', crmRecord)
      //console.log(data)


      const recordInformation = await ZOHO.CRM.API.getRecord({ Entity: crmRecord, RecordID: crmId })
      //console.log(recordInformation.data[0])
      if (recordInformation.data[0].Family_Members.length > 0) {
        this.familyMembers.push(recordInformation.data[0].Family_Members)
        const data = {
          Firts_Name: recordInformation.data[0].Last_Name,
          Phone: recordInformation.data[0].Mobile,
          Address: recordInformation.data[0].Address_Line_1,
          Last_Name: ''
        }
        this.leadData.push(data)

      } else {
        const data = {
          Firts_Name: recordInformation.data[0].Last_Name,
          Phone: recordInformation.data[0].Mobile,
          Address: recordInformation.data[0].Address_Line_1,
          Last_Name: ''
        }
        this.leadData.push(data)
        // console.log(this.leadData)
      }
      /*
                        Client_Name: this.id_lead,
                        Deal_Name: Matter,
                        Matter: this.Matter_ID,
                        Criminal_Department: element.depC,
                        Immigrant_Department: element.depI,
                        Fee: element.fee,
                        Initial_Deposit: element.deposit
      */

      this.Matter_Client_Number = recordInformation.data[0].Matter_Client_Number
      this.Matter_ID = recordInformation.data[0].Account_Name
      //console.log("Matter: ", this.Matter_ID.id)
      //console.log(this.familyMembers[0])
      //console.log(this.Matter_Client_Number)
      /*await ZOHO.CRM.API.getRecord({ Entity: crmRecord, RecordID: crmId })
        .then(function (record) {
          console.log(record.data)
          console.log(record.data[0].Family_Members)
          const dataFamily = record.data[0].Family_Members
          this.familyMembers.push(dataFamily)
          //console.log("family", that.familyMembers)
        })*/

    })
    ZOHO.embeddedApp.init();
  }

};
</script>

<style>
.alert {
  text-align: center;
}
</style>
