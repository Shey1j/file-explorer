<template>
  <SubHeader :header="tableByIdHeader" />
  <div>
    <div class="explorer-table-container">
      <div class="explorer-table-form">
        <div class="explorer-table-control-section">
          <div v-show="showTableById">
            <button class="go-back-link" @click="handleGoBack">Go Back</button>
          </div>
        </div>
        <div class="explorer-table-buttons-section">
          <button class="add-file" @click="(e) => toggleAddModal('File')">
            Add New File
          </button>
          <button class="add-folder" @click="(e) => toggleAddModal('Folder')">
            Add New Folder
          </button>
        </div>
      </div>

      <div class="explorer-table">
        <table>
          <thead>
            <tr>
              <th>S/N</th>
              <th>Name</th>
              <th>Time Added</th>
              <th>Time Modified</th>
              <th />
              <th />
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(data, index) in filteredDataFile"
              :key="index"
              :style="[data.fileType === 'Folder' ? 'cursor: pointer' : null]"
              @click="
                (e) =>
                  handleShowTableById(e, data.id, data.fileType, data.fileName)
              "
            >
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
                  @click.stop="handleShowTableById"
                  @click="(e) => toggleRenameModal(data.fileType, data.id)"
                >
                  Rename
                </button>
              </td>
              <td>
                <button
                  class="delete-button"
                  @click.stop="handleShowTableById"
                  @click="(e) => toggleDeleteModal(data.fileType, data.id)"
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
    <div v-if="showAddModal">
      <AddModal
        :header="addHeader"
        :fileId="addFileIndex"
        :pathList="tableByIdHeader"
        :parentData="dataFile"
        @newFile="addFile"
        @closeAddModal="toggleAddModal"
      />
    </div>
  </div>
</template>

<script>
import RenameModal from "./RenameModal.vue";
import DeleteModal from "./DeleteModal.vue";
import AddModal from "./AddFileOrFolder.vue";
import SubHeader from "./SubHeader.vue";

export default {
  components: { RenameModal, DeleteModal, AddModal, SubHeader },
  data() {
    return {
      header: "",
      dataFile: [],
      filteredDataFile: [],
      fileName: "",
      fileImage: "",
      newFileName: "",
      showAddModal: false,
      showRenameModal: false,
      showDeleteModal: false,
      addHeader: "",
      renameHeader: "",
      deleteHeader: "",
      addFileIndex: 0,
      renameFileIndex: 0,
      deleteFileIndex: 0,
      tableByIdHeader: [],
      showTableById: false,
    };
  },
  methods: {
    handleArraySort(item1, item2) {
      if (item2.fileType > item1.fileType) {
        return 1;
      }
      if (item2.fileType < item1.fileType) {
        return -1;
      }
      return 0;
    },
    addFile(value) {
      for (let i = 0; i < value.length; i++) {
        this.dataFile.push(value[i]);
      }
      if (this.tableByIdHeader.length > 0) {
        const filteredItems = this.dataFile.filter(
          (item) =>
            item.parentId ===
            this.tableByIdHeader[this.tableByIdHeader.length - 1].id
        );
        for (let i = 0; i < filteredItems.length; i++) {
          this.filteredDataFile.push(filteredItems[i]);
          this.filteredDataFile = [...new Set(this.filteredDataFile)];
          this.filteredDataFile.sort(this.handleArraySort);
        }
      } else {
        for (let i = 0; i < this.dataFile.length; i++) {
          {
            this.filteredDataFile.push(this.dataFile[i]);
            this.filteredDataFile = [...new Set(this.filteredDataFile)];
            this.filteredDataFile.sort(this.handleArraySort);
          }
        }
      }
    },
    renameFile(value) {
      for (let i = 0; i < this.dataFile.length; i++) {
        if (value.index === this.dataFile[i].id) {
          this.dataFile[i].fileName = value.newName;
          this.dataFile[i].timeChanged = value.time;
        }
      }
      if (this.tableByIdHeader.length > 0) {
        this.showTableById = true;
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

    toggleAddModal(value) {
      this.addHeader = value;
      this.showAddModal = !this.showAddModal;
    },

    deleteFromFilteredFile(value) {
      for (let item = 0; item < this.filteredDataFile.length; item++) {
        if (value === this.filteredDataFile[item].id) {
          this.filteredDataFile.splice(item, 1);
        }
      }
    },

    deleteFile(value) {
      for (let i = 0; i < this.dataFile.length; i++) {
        if (value === this.dataFile[i].id) {
          for (let j = 0; j < this.dataFile.length; j++) {
            if (value === this.dataFile[j].parentId) {
              this.dataFile.splice(j, 1);
            }
          }
          this.dataFile.splice(i, 1);
        }
        this.deleteFromFilteredFile(value);
      }
      if (this.tableByIdHeader.length > 0) {
        this.showTableById = true;
      }
    },
    handleShowTableById(e, index, type, name) {
      e.preventDefault();
      if (type === "Folder") {
        const data = {
          name: name,
          id: index,
        };
        this.tableByIdIndex = index;
        this.tableByIdHeader.push(data);
        this.filteredDataFile = this.dataFile.filter(
          (item) => item.parentId === index
        );
        this.filteredDataFile.sort(this.handleArraySort);
        this.showTableById = true;
      } else {
        this.showTableById = false;
      }
    },

    handleGoBack() {
      this.tableByIdHeader.pop();
      if (this.tableByIdHeader.length > 0) {
        this.filteredDataFile = this.dataFile.filter(
          (item) =>
            item.parentId ===
            this.tableByIdHeader[this.tableByIdHeader.length - 1].id
        );
        this.filteredDataFile.sort(this.handleArraySort);
      } else {
        this.filteredDataFile = this.dataFile.filter(
          (item) => item.parentId === null
        );
        this.filteredDataFile.sort(this.handleArraySort);
        this.showTableById = false;
      }
    },
  },
};
</script>

<style>
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

.explorer-table-control-section {
  display: flex;
  column-gap: 12px;
  column-gap: 12px;
  justify-content: center;
  align-items: center;
}

.go-back-link {
  border: none;
}

.explorer-table-buttons-section {
  display: flex;
  justify-content: flex-end;
  column-gap: 0.75rem;
  margin: 1rem 0;
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
  border: 1px solid #0e4f8e;
}

.add-file:hover {
  padding: 0.55rem 1.15rem;
}

.add-folder {
  background: #0e4f8e;
  border: none;
  color: #fff;
}

.explorer-table-form input {
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
  background-color: #077bab;
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
  background: #e59e45;
  border: none;
  color: #fff;
}

.delete-button {
  background: #ef3751;
  border: none;
  color: #fff;
}

button:hover {
  color: #fff;
  background: #000;
  border: none;
}
</style>
