<template>
  <v-app>
    <v-main>
      <HeaderApp />
      <FormFamilyVue :Matter_Client_Number="Matter_Client_Number" :id_lead="id_lead" :familyData="familyMembers" />
    </v-main>
  </v-app>
</template>

<script>

import HeaderApp from './components/HeaderApp.vue';
import FormFamilyVue from './components/FormFamily.vue';

export default {
  name: 'App',

  components: {
    FormFamilyVue,
    HeaderApp
  },

  data: () => ({
    familyMembers: [],
    id_lead: '',
    Matter_Client_Number : ''
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
      this.familyMembers.push(recordInformation.data[0].Family_Members)
      this.Matter_Client_Number = recordInformation.data[0].Matter_Client_Number
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
