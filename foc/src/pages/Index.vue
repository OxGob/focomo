<template>
  <q-page class="bg-cyan-2 q-ma-xl">
    <!-- BEGIN Section for trying out tabs and pages -->
    <div>
      <q-tabs align="justify">
        <q-route-tab slot="title" name="tab-1" icon="subject" label="Main P" to="/" exact />
        <q-route-tab slot="title" name="tab-2" icon="fingerprint" label="Test P1" to="/testP1" exact />
      </q-tabs>
    </div>
    <!-- CLOSE Section for trying out tabs and pages -->
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
      opened: true,
      mainObj: ''
    }
  },
  methods: {
    openModal () {
      this.opened = true
    }
  }
}
</script>
