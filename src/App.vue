<script>
import JSConfetti from 'js-confetti'

import DeleteModal from './DeleteModal.vue'
import UpdateModal from './UpdateModal.vue'

const confetti = new JSConfetti()

export default {
  name: "App",
  components: {
    DeleteModal,
    UpdateModal
  },
  data() {
    return {
      enableConfetti: false,
      logs: null,
      selectedLog: null,
      showDeleteModal: false,
      showUpdateModal: false
    };
  },
  methods: {
    async getAllLogs() {
      this.logs = null
      const res = await fetch(`https://vrs-data.azurewebsites.net/api/vrslogentries`)
      //const res = await fetch(`https://localhost:7108/api/vrslogentries`)
      this.logs = await res.json()
    },
    update(log) {
      this.selectedLog = log
      this.showUpdateModal = true
    },
    del(log) {
      this.selectedLog = log
      this.showDeleteModal = true
    },
    deleted() {
      if (this.enableConfetti)
        confetti.addConfetti()

      this.getAllLogs()
      this.showDeleteModal = false
    },
    updated() {
      if (this.enableConfetti)
        confetti.addConfetti()

      this.showUpdateModal = false
    }
  },
  mounted() {
    this.getAllLogs()
  }
};
</script>

<template>
  <div>
    <h1 title="I'm sorry for this 'nice' design, but I've not been touching the front-end dev for more than a year.">VRS
      Data Monitor</h1>
    <div style="margin-top: -20px;">
      <input type="checkbox" id="checkbox" v-model="enableConfetti">
      <label for="checkbox"><span v-if="enableConfetti">confetti enabled :)</span></label>
    </div>
    <p v-if="!logs">Loading...</p>
    <div v-else>
      <div v-for="log in logs" v-bind:key="log.id">
        <ul style="list-style-type:none;">
          <li>IMO: {{ log.imoNumber }}</li>
          <li>Name: {{ log.vesselName }}</li>
          <li>Date: {{ log.date | formatDate }}</li>
          <li>Position: {{ log.position.latitude }} / {{ log.position.longitude }}</li>
          <li>
            <div class="btn-group">
              <button id="show-update-modal" @click="update(log)">Update</button>
              <button id="show-delete-modal" @click="del(log)">Delete</button>
            </div>
          </li>
        </ul>

        <br>
      </div>
    </div>

    <deleteModal :show="showDeleteModal" :log="selectedLog" @close="showDeleteModal = false" @deleted="deleted()">
    </deleteModal>

    <updateModal :show="showUpdateModal" :log="selectedLog" @close="showUpdateModal = false" @updated="updated()">
    </updateModal>
  </div>
</template>

<style>
.btn-group button {
  background-color: #c7c7c7;
  border: 1px solid green;
  padding: 3px 7px;
  margin-right: 10px;
  cursor: pointer;
  float: left;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-footer {
  padding-bottom: 20px;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>