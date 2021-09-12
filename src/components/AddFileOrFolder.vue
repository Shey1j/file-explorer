<template>
  <div class="rename-modal-backdrop">
    <div class="rename-modal">
      <h1>Add New {{ header }}</h1>
      <div class="modal-form">
        <input
          type="text"
          placeholder="Enter name"
          :value="fileName"
          @change="fileInputChange"
        />
        <div class="modal-buttons-section">
          <button class="add-button" @click="handleNameSubmit">Add</button>
          <button class="close-button" @click="handleAddModalClose">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import shortid from "shortid";

export default {
  name: "add-file-or-folder",
  props: { header: String, fileId: Number, pathList: Array, parentData: Array },
  data() {
    return {
      fileName: "",
      timeAdded: "",
      newFile: [],
      parentId:
        this.pathList.length > 0
          ? this.pathList[this.pathList.length - 1].id
          : null,
    };
  },

  methods: {
    fileInputChange(e) {
      const val = e.target.value;
      const date = new Date();
      this.timeAdded = date.toLocaleTimeString("en-US");
      this.fileName = val;
      if (val.length > 0) {
        this.newFile.push({
          id: shortid.generate(),
          fileName: this.fileName,
          fileType: this.header,
          time: this.timeAdded,
          timeChanged: this.timeAdded,
          parentId: this.parentId,
        });
      }
      this.fileName = "";
    },
    handleNameSubmit() {
      this.$emit("newFile", this.newFile);
      this.handleAddModalClose();
    },

    handleAddModalClose() {
      this.$emit("closeAddModal");
    },
  },
};
</script>

<style scoped>
.add-button {
  background: #0e4f8e;
}
</style>