<script>
export default {
  props: {
    show: Boolean,
    log: Object
  },
  methods: {
    async del() {
      fetch(`https://vrs-data.azurewebsites.net/api/vrslogentries/${this.log.id}`, {
        method: 'DELETE'
      })
        .then(
          res => {
            console.log(res.json())
            this.$emit('deleted')
          }
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
            <slot name="header">Delete log ?</slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              <ul style="list-style-type:none;">
                <li>IMO: {{ log.imoNumber }}</li>
                <li>Name: {{ log.vesselName }}</li>
                <li>Date: {{ log.date | formatDate }}</li>
                <li>Position: {{ log.position.latitude }} / {{ log.position.longitude }}</li>
              </ul>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <div class="btn-group" style="float: right;">
                <button @click="$emit('close')">Cancel</button>
                <button style="background-color: #d18888;" @click="del()">Delete</button>
              </div>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>
