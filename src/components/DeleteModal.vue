<template>
    <div class="delete-modal-backdrop">
    <div class="delete-modal">
      <h3>Are you sure you want to delete this {{ header }}?</h3>
        <div class="modal-buttons-section">
          <button class="delete-modal-button" @click="handleFileDelete">
            Delete
          </button>
          <button class="close-button" @click="handleModalClose">Cancel</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["header", "fileId"],
    methods: {
        handleModalClose() {
            this.$emit("closeDelete");
        },

        handleFileDelete() {
            this.$emit("fileDelete", this.fileId);
            this.handleModalClose();
        }
    }
}
</script>

<style>
.delete-modal-backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}

.delete-modal {
  width: 25rem;
  padding: 1.25rem;
  margin: 6.25rem auto;
  background: #fff;
  border-radius: 0.625rem;
  text-align: center;
}

.modal-buttons-section {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2rem;
    column-gap: 0.5rem;
}

.modal-buttons-section button {
  border-radius: 5px;
  font-family: "Karla", sans-serif;
  font-weight: 500;
  padding: 0.5rem 1.25rem;
  cursor: pointer;
  color: #fff;
  border: none;
}

.modal-buttons-section button:hover {
  background: #000;
}

.delete-modal-button {
  background: #0E4F8E;
}

.close-button {
  background: #EF3751;
}
</style>