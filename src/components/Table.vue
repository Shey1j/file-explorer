<template>
  <div class="explorer-table-container">
    <div class="explorer-table-form">
      <div>
        <div v-show="showInput">
          <input
            type="text"
            placeholder="Enter name"
            :value="fileName"
            @change="fileInputChange"
          />
        </div>
      </div>
      <div class="explorer-table-buttons-section">
        <button class="add-file" v-if="!showInput" @click="toggleInput('File')">
          Add New File
        </button>
        <button class="add-file" v-else @click="fileInputChange">
          Add New File
        </button>
        <button
          class="add-folder"
          v-if="!showInput"
          @click="toggleInput('Folder')"
        >
          Add New Folder
        </button>
        <button class="add-folder" v-else @click="fileInputChange">
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
          <tr v-for="(data, index) in dataFile" :key="index">
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
              <button class="delete-button" @click="(e) => toggleDeleteModal(data.fileType, index)">Delete</button>
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
      @newName="renameFile"
      @close="toggleRenameModal"
    />
  </div>
    <div v-if="showDeleteModal">
    <DeleteModal
      :header="deleteHeader"
      :fileId="deleteFileIndex"
      @closeDelete="toggleDeleteModal"
      @fileDelete="deleteFile"
    />
  </div>
</template>

<script>
import RenameModal from "./RenameModal.vue";
import DeleteModal from "./DeleteModal.vue";

export default {
  components: { RenameModal, DeleteModal },
  data() {
    return {
      header: "",
      showInput: false,
      dataFile: [],
      fileName: "",
      fileImage: "",
      newFileName: "",
      showRenameModal: false,
      showDeleteModal: false,
      renameHeader: "",
      deleteHeader: "",
      renameFileIndex: 0,
      deleteFileIndex: 0,
    };
  },
  methods: {
    toggleInput(value) {
      this.header = value;
      this.showInput = !this.showInput;
    },

    fileInputChange(e) {
      const date = new Date();
      this.time = date.toLocaleTimeString("en-US");
      this.fileName = e.target.value;
      if (this.header === "Folder") {
        this.dataFile.unshift({
          id: this.dataFile.length - this.dataFile.length,
          fileName: this.fileName,
          fileType: this.header,
          time: this.time,
          timeChanged: this.time
        });
      } else {
        this.dataFile.push({
          id: this.dataFile.length - 1,
          fileName: this.fileName,
          fileType: this.header,
          time: this.time,
          timeChanged: this.time
        });
      }
      this.fileName = "";
      this.toggleInput();
    },

    renameFile(value) {
      for (let i = 0; i < this.dataFile.length; i++) {
        if (value.index === i) {
          this.dataFile[i].fileName = value.newName;
          this.dataFile[i].timeChanged = value.time;
        }
      }
    },
    toggleRenameModal(value, id) {
      this.renameHeader = value;
      this.renameFileIndex = id;
      this.showRenameModal = !this.showRenameModal;
    },

    toggleDeleteModal(value, id) {
      this.deleteHeader = value;
      this.deleteFileIndex = id;
        this.showDeleteModal = !this.showDeleteModal;
    },

    deleteFile(value) {
        for (let i = 0; i < this.dataFile.length; i++) {
            if (value === i) {
                this.dataFile.splice(i, 1);
            }
        }
    }
  },
};
</script>

<style scoped>
.explorer-table-container {
  margin: 0 auto;
  width: 60%;
  font-family: "Karla", sans-serif;
}

.explorer-table-form {
  display: flex;
  padding: 1.25rem;
  justify-content: space-between;
}

.explorer-table-buttons-section {
  display: flex;
  justify-content: flex-end;
  column-gap: 0.75rem;
}

button {
  border-radius: 5px;
  font-family: "Karla", sans-serif;
  font-weight: 500;
  padding: 0.5rem 1.25rem;
  cursor: pointer;
}

.add-file {
  background: transparent;
  border: 1px solid #0E4F8E;
}

.add-file:hover {
  padding: 0.55rem 1.15rem;
}

.add-folder {
  background: #0E4F8E;
  border: none;
  color: #fff;
}

input {
  width: 12rem;
  height: 2.5rem;
  padding-left: 1rem;
  border-radius: 5px;
  border: 1px solid grey;
  font-family: "Karla", sans-serif;
}

table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #ddd;
}

th,
td {
  height: 2.5rem;
  padding-left: 1rem;
}

th {
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
  text-align: left;
  background-color: #077BAB;
  color: white;
}

td {
  border-bottom: 1px solid #ddd;
}

.table-data {
  display: flex;
  align-items: center;
}

.table-data img {
  width: 1.125rem;
  height: 1.125rem;
}

.rename-button {
  background: #E59E45;
  border: none;
  color: #fff;
}

.delete-button {
  background: #EF3751;
  border: none;
  color: #fff;
}

button:hover {
  color: #fff;
  background: #000;
  border: none;
}
</style>