<template>
  <q-page class="bg-cyan-2 q-ma-xl">
    <!-- Added button to open modal -->
    <q-btn label="Open Modal" color="purple" @click="openModal" />
     <q-modal v-model="opened">
       <testCoVi></testCoVi>
        <q-btn
          color="primary"
          @click="opened = false"
          label="Exit"
        />
      </q-modal>
  </q-page>
</template>

<script>
import testCompoV from 'components/testCoVi.vue'
import testCompoBu from 'components/testCoBu.vue'
export default {
  components: {
    'testCoVi': testCompoV,
    'testCoBu': testCompoBu
  },
  data () {
    return {
      opened: true
    }
  },
  methods: {
    openModal () {
      this.opened = true
    }
  }
}
</script>
