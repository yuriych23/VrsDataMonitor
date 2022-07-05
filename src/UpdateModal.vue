<script>
export default {
  props: {
    show: Boolean,
    log: Object
  },
  methods: {
    async update() {
      fetch(`https://vrs-data.azurewebsites.net/api/vrslogentries`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.log)
      })
        .then(
          this.$emit('updated')
        )
    }
  }
}
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header"></slot>
          </div>

          <div class="modal-body">
            Vessel Name:
            <br>
            <input v-model="log.vesselName" />
            <br><br>
            Latitude:
            <br>
            <input v-model="log.position.latitude" />
            <br><br>
            Longitude:
            <br>
            <input v-model="log.position.longitude" />
            <br><br>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <div class="btn-group" style="float: right;">
                <button @click="$emit('close')">Cancel</button>
                <button style="background-color: #99d188;" @click="update()">Update</button>
              </div>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>
