<script>
import DeleteModal from './DeleteModal.vue'
import UpdateModal from './UpdateModal.vue';

export default {
  name: "App",
  components: {
    DeleteModal,
    UpdateModal
  },
  data() {
    return {
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
      this.getAllLogs()
      this.showDeleteModal = false
    },
    updated() {
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
    <h1>VRS Logs</h1>
    <p v-if="!logs">Loading...</p>
    <pre v-else>
      <div v-for="log in logs" v-bind:key="log.id">
        IMO: {{ log.imoNumber }}
        Name: {{ log.vesselName }}
        Date: {{ log.date }}
        Position: {{ log.position.latitude }} / {{ log.position.longitude }}

        <button id="show-update-modal" @click="update(log)">Update</button>
        <button id="show-delete-modal" @click="del(log)">Delete</button>

      </div>
    </pre>
  
    <deleteModal :show="showDeleteModal" :log="selectedLog" @close="showDeleteModal = false" @deleted="deleted()">
    </deleteModal>

    <updateModal :show="showUpdateModal" :log="selectedLog" @close="showUpdateModal = false" @updated="updated()">
    </updateModal>
  </div>
</template>

<style>

</style>