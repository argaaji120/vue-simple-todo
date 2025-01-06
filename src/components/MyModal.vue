<template>
  <div class="modal fade" :id="modalId" tabindex="-1" ref="modal">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ title }}</h5>
          <button type="button" class="btn-close" v-on:click="handleClose"></button>
        </div>
        <div class="modal-body">
          <slot name="body"></slot>
        </div>
        <div class="modal-footer">
          <slot name="footer"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from 'bootstrap'

export default {
  props: {
    title: {
      type: String,
      default: 'Modal Title',
    },
    modalId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      modalInstance: null,
    }
  },
  mounted() {
    // Initialize Bootstrap Modal
    this.modalInstance = new Modal(this.$refs.modal)
  },
  methods: {
    openModal() {
      if (this.modalInstance) {
        this.modalInstance.show()
      }
    },
    closeModal() {
      if (this.modalInstance) {
        this.modalInstance.hide()
      }
    },
    handleClose() {
      this.$emit('close')
    },
  },
}
</script>
