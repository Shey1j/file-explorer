<template>
  <div class="explorer-table-container">
    <div class="explorer-table-form">
      <div class="explorer-table-control-section">
        <button class="go-back-link" @click="goBack">Go Back</button>
        <div v-show="showSubInput">
          <input
            type="text"
            placeholder="Enter name"
            :value="subFileName"
            @change="subFileInputChange"
          />
        </div>
      </div>
      <div class="explorer-table-buttons-section">
        <button class="add-file" v-if="!showSubInput" @click="toggleSubInput('File')">
          Add New File
        </button>
        <button class="add-file" v-else @click="subFileInputChange">
          Add New File
        </button>
        <button
          class="add-folder"
          v-if="!showSubInput"
          @click="toggleSubInput('Folder')"
        >
          Add New Folder
        </button>
        <button class="add-folder" v-else @click="subFileInputChange">
          Add New Folder
        </button>
      </div>
    </div>
    <div class="explorer-table">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Time Added</th>
            <th>Time Modified</th>
            <th />
            <th />
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in subFileData" :key="index">
            <td>{{ index + 1 }}</td>
            <td>
              <div class="table-data">
                <img
                  v-if="data.fileType === 'Folder'"
                  src="../assets/folder.png"
                  :alt="data.fileType"
                /><img v-else src="../assets/file.png" :alt="data.fileType" />
                {{ data.fileName }}
              </div>
            </td>
            <td>{{ data.time }}</td>
            <td>{{ data.timeChanged }}</td>
            <td>
              <button
                class="rename-button"
                @click="(e) => toggleRenameModal(data.fileType, index)"
              >
                Rename
              </button>
            </td>
            <td>
              <button
                class="delete-button"
                @click="(e) => toggleDeleteModal(data.fileType, index)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <div v-if="showRenameModal">
    <RenameModal
      :header="renameHeader"
      :fileId="renameFileIndex"
      @newName="renameSubFile"
      @close="toggleRenameModal"
    />
  </div>
  <div v-if="showDeleteModal">
    <DeleteModal
      :header="deleteHeader"
      :fileId="deleteFileIndex"
      @closeDelete="toggleDeleteModal"
      @fileDelete="deleteSubFile"
    />
  </div>
</template>

<script>
import RenameModal from "./RenameModal.vue";
import DeleteModal from "./DeleteModal.vue";

export default {
  components: { RenameModal, DeleteModal },
  props: {"header": String, "fileId": Number, "parentData": Array},
  data() {
    return {
      subHeader: "",
      showSubInput: false,
      subFileData: this.parentData[this.fileId].subFile,
      subFileName: "",
      subFileImage: "",
      showRenameModal: false,
      showDeleteModal: false,
      renameHeader: "",
      deleteHeader: "",
      renameFileIndex: "",
      deleteFileIndex: "",
    };
  },

  methods: {
    toggleSubInput(value) {
      this.subHeader = value;
      this.showSubInput = !this.showSubInput;
    },

    subFileInputChange(e) {
      const date = new Date();
      this.time = date.toLocaleTimeString("en-US");
      this.subFileName = e.target.value;
      if (this.subHeader === "Folder") {
        this.subFileData.unshift({
          id: this.subFileData.length - this.subFileData.length,
          parentId: this.fileId,
          fileName: this.subFileName,
          fileType: this.subHeader,
          time: this.time,
          timeChanged: this.time,
        });
      } else {
        this.subFileData.push({
          id: this.subFileData.length - 1,
          parentId: this.fileId,
          fileName: this.subFileName,
          fileType: this.subHeader,
          time: this.time,
          timeChanged: this.time,
        });
      }
      this.subFileName = "";
      this.toggleSubInput();
    },

    toggleRenameModal(value, id) {
        this.renameHeader = value;
        this.renameFileIndex = id;
        this.showRenameModal = !this.showRenameModal;
    },

    renameSubFile(value) {
      for (let i = 0; i < this.subFileData.length; i++) {
        if (value.index === i) {
          this.subFileData[i].fileName = value.newName;
          this.subFileData[i].timeChanged = value.time;
        }
      }
    },

    toggleDeleteModal(value, id) {
      this.deleteHeader = value;
      this.deleteFileIndex = id;
        this.showDeleteModal = !this.showDeleteModal;
    },

    deleteSubFile(value) {
        for (let i = 0; i < this.subFileData.length; i++) {
            if (value === i) {
                this.subFileData.splice(i, 1);
            }
        }
    },

    goBack() {
        this.$emit("subFile", this.subFileData);
        this.$emit("back");
    }
  },
};
</script>

<style scoped>
.explorer-table-control-section {
  display: flex;
  column-gap: 12px;
}

.go-back-link {
    border: none;
}
</style>