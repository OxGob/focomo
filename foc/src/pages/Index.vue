<template>
  <q-page class="bg-cyan-2 q-ma-xl" :content-css="{minWidth: '50vw'}">
     <q-modal v-model="opened">
        <h4>Basic Modal</h4>
        <q-btn
          color="primary"
          @click="opened = false"
          label="Close"
        />
      </q-modal>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      opened: true
    }
  }
}
</script>
