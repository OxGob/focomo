<template>
  <q-page padding>
    <div>
      <q-tabs align="justify">
        <q-route-tab slot="title" name="tab-1" icon="subject" label="Main P" to="/" exact />
        <q-route-tab slot="title" name="tab-2" icon="fingerprint" label="Test P1" to="/testP1" exact />
      </q-tabs>
    </div>
    <h6> This is testPg1</h6>
    <q-field label="Title Name: ">
      <q-input
      v-model= "nameTitle"
       placeholder="Enter the name" />
    </q-field>
    <!-- content -->
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      nameTitle: ''
    }
  }
}
</script>
