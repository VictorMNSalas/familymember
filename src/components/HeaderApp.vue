<template>
  <v-app-bar color="blue" density="compact">
    <div id="header">
      <h1 class="title">Retainers</h1>
      <v-icon id="close_icon" @click="closeWisget()"> {{ CloseIcon }}</v-icon>
    </div>

  </v-app-bar>
</template>

<script>
import { mdiWindowClose } from '@mdi/js';
export default {
  components: {
  },
  data() {
    return {

    }
  },
  computed: {
    CloseIcon() {
      return mdiWindowClose;
    }
  },
  methods: {
    /* eslint-disable */
    closeWisget() {
      ZOHO.CRM.UI.Popup.close()
        .then(function (data) {
          console.log(data)
        })
    }
  }


}
</script>

<style>
#header {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.title {
  margin: 0 2rem;
  color: white;
}

#close_icon {
  cursor: pointer;
  color: white;
}

#close_icon:hover {
  color: red;
}
</style>