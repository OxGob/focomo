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
    <q-card class="bg-cyan-2 q-ma-xl">
      <q-card-main>
        <!-- Form Section -->
        <q-btn label="Add Form" icon="add" color = "primary" @click="addFormTapped"/>
        <q-card-separator class="q-mb-sm q-mt-md"/>
        <div v-for="(formG, formInd) in formsGen" :key=formG.id>
          <div v-if="formG.rowExists">
            <div class="q-mb-md q-mt-md" v-show="showRowFirstTime">
                <div class="row">
                  <div class="col-8"></div>
                  <div class="col-4">
                    <q-btn class="float-right q-mb-sm" color = "pink" icon="remove" label="Remove Form" @click="removeForm(formInd)"/>
                  </div>
                </div>
                <q-field class="q-mb-sm" label="Form Label: ">
                  <q-input v-model="formG.formLabel" type="text" align="center" clearable />
                </q-field>
                <div class="row">
                  <div class="col-6">
                    <q-btn class="q-mt-sm" color="purple" icon-right="open_in_new" label="Open Form Builder" @click="openBuilder(formInd)" />
                  </div>
                  <div class="col-6">
                    <div v-show="formG.showFormViewerBtn">
                      <q-btn class="float-right q-mt-sm" color="green-4" icon-right="open_in_new" label="Open Form Viewer" @click="goToViewer(formInd)" />
                    </div>
                  </div>
                </div>
          <!-- Form Builder Section -->
              <!-- Get flag from emitOpenFormViewer in Form Builder to show formViewer. -->
              <div v-show="formG.openedBu">
                  <q-modal v-model="formG.openedBu">
                      <div class="row">
                        <div class="col-6"></div>
                        <div class="col-6">
                          <q-btn class="float-right q-mr-sm q-mt-sm" color="red" icon="home" label="Exit Form Builder" @click="exitBuilder(formInd)"/>
                        </div>
                      </div>
                        <coBuild @chiObjForm="formG.formComponentObj = $event"
                         @emitOpenFormViewer="goToViewer(formInd)"></coBuild>
                  </q-modal>
              </div>
          <!-- Form Viewer Section -->
              <!-- Use v-if so that on the creation of component formViewer, init() can be run in formViewer -->
              <!-- On returning to its parent, the formViewer component will be destroyed -->
              <div v-if="formG.openedVi">
                <!-- Send to component prop (:valFromParent). Receive from child (@returnToParent)   -->
                <q-modal v-model="formG.openedVi">
                  <div class="row">
                    <div class="col-8"></div>
                    <div class="col-4">
                      <q-btn class="float-right q-mr-sm q-mt-sm" color = "red" icon="home" label="Exit All" @click="exitViewer(formInd)"/>
                    </div>
                  </div>
                  <coView :valFromParent='formG' @returnToParent="backToBuilder(formInd)" @exitViewerFormFinished="exitViewer(formInd)"></coView>
                </q-modal>
              </div>
              <q-card-separator class="q-mb-md q-mt-lg"/>
            </div>
          </div>
        </div>
      </q-card-main>
    </q-card>
  </q-page>
</template>

<script>
import testCompoV from 'components/testCoVi.vue'
import testCompoBu from 'components/testCoBu.vue'
import compBu from 'components/coBuilder.vue'
import compVi from 'components/coViewer.vue'
export default {
  components: {
    'coBuild': compBu,
    'coView': compVi,
    'testCoVi': testCompoV,
    'testCoBu': testCompoBu
  },
  data () {
    return {
      counterformsGen: 0,
      removedAllForms: false,
      showRowFirstTime: false,
      formsGen: [
        {
          formLabel: 'form 0',
          formComponentObj: '',
          indexFo: 0,
          rowExists: true,
          openedBu: false,
          openedVi: false,
          showFormViewerBtn: false
        }
      ],
      formTracker: [
        {
          formTkID: 'form 0',
          indexOfForm: 0
        }
      ]
    }
  },
  methods: {
    // Form Row section
    addFormTapped () {
      var formInd = this.formsGen.length - 1
      if (this.removedAllForms === true) {
        this.addFormRow()
        var formIndRAll = this.formsGen.length - 1
        this.openBuilder(formIndRAll)
      } else {
        if (this.counterformsGen === 0) {
          this.showRowFirstTime = true
          this.openBuilder(formInd)
        } else if (this.counterformsGen > 0) {
          this.addFormRow()
          var formInd1 = this.formsGen.length - 1
          this.openBuilder(formInd1)
        }
      }
      this.removedAllForms = false
      this.counterformsGen++
    },
    removeForm (formInd) {
      if (formInd === 0) {
        this.$q.notify('Removing last form')
        this.formsGen[formInd].rowExists = false
        this.formsGen.splice(formInd, 1)
        this.removedAllForms = true
        this.updtFormTracker(formInd)
      } else {
        this.formsGen[formInd].rowExists = false
        this.formsGen.splice(formInd, 1)
        this.updtFormTrackerRemove(formInd)
      }
    },
    addFormRow () {
      this.formsGen.push({
        formLabel: 'Form ' + this.counterformsGen,
        formComponentObj: '',
        indexFo: this.counterformsGen,
        rowExists: true,
        openedBu: false,
        openedVi: false,
        showFormViewerBtn: false
      })
      this.updtFormTrackerAdd()
    },
    updtFormTracker (formInd) {
      // at index 0
      this.formTracker[formInd].formTkID = ''
      this.formTracker[formInd].indexOfForm = formInd
    },
    updtFormTrackerAdd () {
      var lastIndexFormObj = this.formsGen.length - 1
      if (this.removedAllForms === false) {
        this.formTracker.push({
          formTkID: this.formsGen[lastIndexFormObj].formLabel,
          indexOfForm: lastIndexFormObj
        })
      } else {
        this.formTracker[0].formTkID = this.formsGen[lastIndexFormObj].formLabel
        this.formTracker[0].indexOfForm = lastIndexFormObj
      }
    },
    updtFormTrackerRemove (formInd) {
      var foTk = this.formTracker
      // remove the selected index from the form tracking Array
      foTk.splice(formInd, 1)
      // In the tracking array, update the form index for those removed AFTER SPLICE index
      var lenTrkAfterSplice = foTk.length
      if (formInd < lenTrkAfterSplice) {
      // If formInd is NOT LAST, then from formInd position till last, subtract 1 from values of indexOfForm
        var i
        for (i = formInd; i < lenTrkAfterSplice; i++) {
          var fn = foTk[i].indexOfForm - 1
          foTk[i].indexOfForm = fn
        }
      }
    },
    // Navigation methods for Builder and Viewer
    // This function returns to the Builder modal.
    backToBuilder (index) {
      this.formsGen[index].openedVi = false
      this.formsGen[index].openedBu = true
    },
    // This function shows the Viewer modal.
    goToViewer (index) {
      this.formsGen[index].showFormViewerBtn = true
      this.formsGen[index].openedBu = false
      this.formsGen[index].openedVi = true
    },
    // This function exits the Builder.
    exitBuilder (index) {
      // Check the builder is the compObj is empty. If yes, open dialog to make sure we want to proceed.
      if (this.formsGen[index].formComponentObj === '') {
        this.$q.dialog({title: 'Exit Confirmation',
          message: 'You have not saved any work yet. Do you want still want to exit?',
          ok: 'Yes, Exit Form Builder',
          cancel: 'No, Stay',
          color: 'secondary',
          preventClose: true}).then(() => {
          this.formsGen[index].openedBu = false
          this.removeForm(index)
        })
      } else {
        this.formsGen[index].openedBu = false
      }
    },
    // This function shows the Builder modal.
    openBuilder (index) {
      this.formsGen[index].openedBu = true
    },
    // This function exits the Viewer.
    exitViewer (index) {
      this.formsGen[index].openedVi = false
    }
  }
}
</script>
