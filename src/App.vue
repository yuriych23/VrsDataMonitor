<template>
  <div>
    <h1>VRS Logs</h1>
    <p v-if="!logs">Loading...</p>
    <pre v-else>{{ logs }}</pre>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      value: "World",
      message: "",
      todoId: 1,
      logs: null
    };
  },
  methods: {
    async getAllLogs() {
      this.logs = null
      const res = await fetch(`https://vrs-data.azurewebsites.net/api/vrslogentries`)
      this.logs = await res.json()
    }
  },
  mounted() {
    this.getAllLogs()
  }
};
</script>
