<template>
  <v-app>
    <v-main>
      <HeaderApp />
      <FormFamilyVue :familyData="familyMembers" />
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
    familyMembers: []
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
    const that = this
    ZOHO.embeddedApp.on("PageLoad", function (data) {

      const crmId = data.EntityId
      const crmRecord = data.Entity
      console.log('id', crmId)
      console.log('modulo', crmRecord)
      //console.log(data)


      ZOHO.CRM.API.getRecord({ Entity: crmRecord, RecordID: crmId })
        .then(function (record) {
          console.log(record.data)
          console.log(record.data[0].Family_Members)
          const dataFamily = record.data[0].Family_Members
          that.familyMembers.push(dataFamily)
          console.log("family", that.familyMembers)
        })
    })
    ZOHO.embeddedApp.init();
  }

};
</script>
