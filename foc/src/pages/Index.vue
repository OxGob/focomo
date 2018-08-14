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
    <q-btn label="Open Modal" color="purple" @click="openBuilder" />
     <q-modal v-model="openedBu">
        <div class="row">
          <div class="col-6"></div>
          <div class="col-6">
            <q-btn class="float-right q-mr-sm q-mt-sm" color="red" label="Exit Bu" @click="exitBuilder"/>
          </div>
        </div>
        <testCoBu></testCoBu>
        <div class="row">
          <div class="col-6"></div>
          <div class="col-6">
            <q-btn class="float-right q-mr-sm q-mb-sm" color="green" label="Go to View" @click="goToView"/>
          </div>
        </div>
     </q-modal>
     <q-modal v-model="openedVi">
       <testCoVi></testCoVi>
        <div class="row">
          <div class="col-6">
            <q-btn class="q-ml-sm q-mb-sm" color="amber" label="Back to Builder" @click="backToBuilder"/>
          </div>
          <div class="col-6">
            <q-btn class="float-right q-mr-sm q-mb-sm" color="red" label="Exit Vi" @click="exitView"/>
          </div>
        </div>
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
      openedBu: true,
      openedVi: false,
      mainObj: ''
    }
  },
  methods: {
    backToBuilder () {
      this.openedVi = false
      this.openedBu = true
    },
    goToView () {
      this.openedBu = false
      this.openedVi = true
    },
    exitBuilder () {
      this.openedBu = false
    },
    openBuilder () {
      this.openedBu = true
    },
    exitView () {
      this.openedVi = false
    }
  }
}
</script>
