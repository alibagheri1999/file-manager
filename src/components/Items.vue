<template>
  <div class="file-item">
    <div @contextmenu="handleRightClick" class="folders">
      <img class="images" v-if="this.File.children.length"
           :src="this.File.isExtended ? '/img/minus.svg': '/img/plus.svg'" alt="plus image" @click="toggle">
      <img class="images" v-else :src="'/img/empty.svg'" alt="plus image" @click="toggle">
      <img class="images" :src="this.File.type === 'folder' ? '/img/folder.svg' : '/img/pdf.svg'" alt="folder image">
      <div class="name">{{ this.File.name }} ({{ new Date(this.File.date).toISOString() }})</div>
    </div>
    <div class="child-list" v-if="this.File.isExtended && this.File.children.length">
      <Items v-for="file in this.File.children" :key="file.date" :file="file"/>
    </div>
    <div v-if="showContextMenu" class="context-menu"
         :style="{ top: contextMenuPosition.top + 'px', left: contextMenuPosition.left + 'px' }">
      <ul>
        <li @click="handleContextOption('add')">Add</li>
        <li @click="handleContextOption('copy')">Copy</li>
        <li @click="handleContextOption('cut')">Cut</li>
        <li @click="handleContextOption('paste')">Paste</li>
        <li @click="handleContextOption('delete')">Delete</li>
      </ul>
    </div>
  </div>
  <div v-if="isUploadModalOpen" class="pdf-sharing">
    <div class="pdf-sharing__upload" v-if="!loading">
      <button :style="{border: (is_dragover || isFileUploaded) ? 'dashed 0.3rem darkBlue' : 'dashed 0.2rem #1a73e8',height: '10rem',width:'20rem','background-color': (is_dragover || isFileUploaded) ? '#b2dcf1' : '#d4eaf5','-webkit-box-shadow': is_dragover ?  '-1px 6px 15px -4px rgba(0,0,0,0.62)':'',
  'box-shadow': (is_dragover) ?  '-1px 6px 15px -4px rgba(0,0,0,0.62)' : ''}">

        <span v-if="!isFileUploaded">upload-pdf'</span>
        <span v-else style="color: limegreen">'file-selected</span>
      </button>

      <input
          @dragenter="is_dragover = true"
          @dragleave="is_dragover = false"
          @click="is_dragover = false"
          type="file"
          name="file"
          ref="file"
          accept=".pdf"
          @change="uploadFile"
      />
    </div>
    <div v-else>
      <base-modal :open="isLoading" :is-loading="'isLoading'" page="conference" modal-color="transparent">
        <template #modal-body>
          <base-loading-spinner size-ratio="1.2" spinner-color="#4fd5ca">
            <template #loading-content>
              <div class="loading__extra-content--text" style="color: #4fd5ca">wait-uploading</div>
            </template>
          </base-loading-spinner>
        </template>
      </base-modal>
    </div>

    <div style="display: flex">
      <base-button
          :is-active="true"
          :button-inner-txt="'pdf-sharing'"
          :continuous-params="
          [
          'background-color=#95d1cc',
          'height=4.1rem',
          'color=white',
          'margin-top=10px'
          ]"
          :style-types="sharePDFBtnStyle"
          @click="startPDFSharing"
      >
      </base-button>
      <base-button
          :is-active="true"
          :button-inner-txt="'cancel'"
          :continuous-params="
          [
          'background-color=#ea0505',
          'height=4.1rem',
          'color=white',
          'margin-top=10px',
          'cursor=pointer'
          ]"
          :style-types="sharePDFBtnStyle"
          @click="stopPDFSharing"
      >
      </base-button>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import Items from "@/components/Items";
import BaseButton from "@/components/BaseButton";
import BaseModal from "@/components/BaseModal";
import BaseLoadingSpinner from "@/components/BaseLoadingSpinner";

export default {

  name: 'TheItems',
  props: ["file"],
  data() {
    return {
      name: "name",
      src: "/img/plus.svg",
      File: this.file,
      showContextMenu: false,
      contextMenuPosition: {top: 0, left: 0},
      selectedOption: '',
      isUploadModalOpen: false,
      isFileUploaded: false,
      is_dragover: false,
      fileReader: null,
      loading: false,
      isEnableDownload: false,
      isLoading: false,
    }
  },
  watch: {},
  components: {
    Items, BaseButton, BaseModal, BaseLoadingSpinner
  },
  computed: {
    sharePDFBtnStyle() {
      const styleFlags = ["button--curved", "'login--button'"];
      if (!this.isFileUploaded) {
        styleFlags.push("button--disabled");
      }
      return styleFlags;
    },
  },
  methods: {
    toggle() {
      this.File.isExtended = !this.File.isExtended
      console.log(this.File)
    },
    handleRightClick(event) {
      event.preventDefault();
      this.showContextMenu = false;
      this.showContextMenu = true;
      this.contextMenuPosition = {top: event.clientY, left: event.clientX};
    },
    handleContextOption(option) {
      console.log('Selected option:', option);
      this.showContextMenu = false;
      switch (option) {
        case "add":
          console.log("add")
            if (this.File.type === "folder"){
              this.isUploadModalOpen = true
            }
          break;
        default:
          break
      }
    },
    async startPDFSharing() {
      this.isLoading = true
      if (this.isFileUploaded) {
        const formData = new FormData();
        const file = this.$refs.file;
        console.info(file.files);
        if (file.files[0]?.type !== "application/pdf") {
          this.isFileUploaded = false
          return;
        }
        formData.append("file", file.files[0]);
        this.loading = true;
        console.log(file.files[0])
        const newData = {
          name: file.files[0].name,
          date: file.files[0].lastModifiedDate,
          type: 'pdf',
          children: [],
          isExtended: false,
        }
        console.log(newData)
        this.File.children.push(newData)
        this.isUploadModalOpen = false
        this.isFileUploaded = false
        this.is_dragover = false
        this.fileReader = null
        this.loading = false
        this.isEnableDownload = false
        this.isLoading = false
      }
    },
    stopPDFSharing(){
      this.isUploadModalOpen = false
      this.isFileUploaded = false
      this.is_dragover = false
      this.fileReader = null
      this.loading = false
      this.isEnableDownload = false
      this.isLoading = false
    },
    uploadFile() {
      this.isFileUploaded = true;
      this.uploadFile = false;
    },
  }
}
</script>

<style scoped>
h1 {
  color: wheat;
}

.folders {
  display: flex;
  flex-direction: row;
}

.images {
  width: 24px;
  height: 24px;
  margin-left: 10px;
}

.name {
  color: white;
  text-align: left;
  margin-left: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.file-item {
  display: flex;
  flex-direction: column;
}

.child-list {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  padding: 0;
  margin-left: 35px;
}

.context-menu {
  position: fixed;
  z-index: 9999;
  background-color: #FFF;
  border: 1px solid #DDD;
  padding: 10px;
}

.context-menu ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.context-menu li {
  padding: 5px 15px;
  cursor: pointer;
}

.context-menu li:hover {
  background-color: #F0F0F0;
}

video {
  width: 100%;
  height: 100%;
}

.pdf-sharing {
  width: 40rem;
  /*height: 5rem;*/
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  position: absolute;
  right: 7%;
  top: 20%;
  /*background-color: white;*/
}

.pdf-sharing__upload {
  width: 100%;
  position: relative;
  cursor: pointer;
  display: flex;
  justify-content: center;
}

.pdf-sharing__upload > input {
  width: 20rem;
  height: 100%;
  opacity: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  cursor: pointer;
  background-color: #d4eaf5;

}

.change-drag-status {
  background: red;
}

.drag {
  background: blue;
}
</style>
