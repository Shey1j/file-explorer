<template>
  <div class="rename-modal-backdrop">
    <div class="rename-modal">
      <h1>Rename {{ header }}</h1>
      <div class="modal-form">
        <input type="text" :value="newName" @change="handleNameChange" />
        <div class="modal-buttons-section">
          <button class="rename-button" @click="handleNewNameSubmit">
            Rename
          </button>
          <button class="close-button" @click="handleModalClose">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["header", "fileId"],
  data() {
    return {
      newName: "",
      timeChange: "",
    };
  },
  methods: {
    handleNameChange(e) {
      const val = e.target.value;
      this.newName = val;
    },
    handleNewNameSubmit() {
      const time = new Date();
      this.timeChange = time.toLocaleTimeString("en-US");
      this.$emit("newName", {
        newName: this.newName,
        index: this.fileId,
        time: this.timeChange,
      });
      this.handleModalClose();
    },
    handleModalClose() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.rename-modal-backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}

.rename-modal {
  width: 25rem;
  padding: 1.25rem;
  margin: 6.25rem auto;
  background: #fff;
  border-radius: 0.625rem;
  text-align: center;
}

.modal-form {
  margin-top: 2rem;
}

.modal-form input {
  width: 15rem;
  height: 2.5rem;
  padding-left: 1rem;
  border-radius: 5px;
  border: 1px solid grey;
  font-family: "Karla", sans-serif;
  margin-bottom: 1rem;
}

.modal-buttons-section {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  column-gap: 12px;
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

.rename-button {
  background: #0E4F8E;
}

.close-button {
  background: #EF3751;
}
</style>