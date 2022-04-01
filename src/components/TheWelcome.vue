<script setup>
import WelcomeItem from "./WelcomeItem.vue";
import DocumentationIcon from "./icons/IconDocumentation.vue";
import ToolingIcon from "./icons/IconTooling.vue";
import EcosystemIcon from "./icons/IconEcosystem.vue";
import CommunityIcon from "./icons/IconCommunity.vue";
import SupportIcon from "./icons/IconSupport.vue";
import { Jomini } from "jomini";
import { ref } from "vue";
</script>

<script>
export default {
   data() {
      return {
         file: "",
         fileName: "",
         fileContent: "",
         parsedContent: "",
         link: "",
         // State Management
         isFileReady: false,
         dragging: false,
      };
   },
   methods: {
      // Watching on input file change
      onFileChanged($event) {
         // Resetting file readiness status with each new file upload
         this.isFileReady = false;

         // Getting file content
         const target = $event.target;
         // Security
         if (target && target.files) {
            this.file = target.files[0];
            console.log(this.file);
         }
         // Changing the filename
         this.fileName = this.file.name.slice(0, -3) + "json";

         // console.log(this.fileName);
         this.readAndParseFile(this.file);
      },
      // Read and parse file input
      readAndParseFile(textFile) {
         // Instancing a vanilla JS filereader
         let reader = new FileReader();
         // Reads the input file as text
         reader.readAsText(textFile);

         // Waits for the loading of the text file and executes the parsing as well as handling page state management
         reader.addEventListener("load", async () => {
            // Get the file txt content
            this.fileContent = reader.result;

            // Parse
            const parser = await Jomini.initialize();
            const parsedData = parser.parseText(this.fileContent);

            // Assign internal variable with the parsed data
            this.parsedContent = parsedData;

            this.link = `data:text/plain;charset=utf-8,${encodeURIComponent(
               JSON.stringify(this.parsedContent, null, 2)
            )}`;

            // console.log(parsedData);
            console.log(this.parsedContent);

            // Displaying the button to download the file
            this.isFileReady = true;
         });
      },
      // Download json file
      downloadJSONFile(filename, text) {
         console.log("hi there");
      },
   },
   // computed: {
   //    extension() {
   //       return this.file ? this.file.name.split(".").pop() : "";
   //    },
   // },
};
</script>

<template>
   <WelcomeItem>
      <template #icon>
         <DocumentationIcon />
      </template>
      <template #heading>Use</template>

      Drag and drop a file below and download the json parsed file.

      <div v-if="!file" class="">
         <div
            :class="['dropZone', dragging ? 'dropZone-over' : '']"
            @dragenter="dragging = true"
            @dragleave="dragging = false"
         >
            <div class="dropZone-info" @drag="onFileChanged($event)">
               <span class="dropZone-title"></span>
               <span class="dropZone-title">Drop file or click to upload</span>
               <div class="dropZone-upload-limit-info">
                  <div>extension support: txt</div>
               </div>
            </div>
            <input type="file" @change="onFileChanged($event)" accept=".txt" />
         </div>
      </div>
      <div v-else class="dropZone-uploaded">
         <a
            v-if="isFileReady"
            :href="link"
            :download="fileName"
            class="download-anchor"
         >
            <button v-if="isFileReady" class="download-json-file">
               Your file is ready, click to download !
            </button>
         </a>
         <div v-if="isFileReady" class="new-file-upload">
            <label>Upload another file</label>
            <input type="file" @change="onFileChanged($event)" accept=".txt" />
         </div>
      </div>
   </WelcomeItem>

   <WelcomeItem>
      <template #icon>
         <ToolingIcon />
      </template>
      <template #heading>Tooling</template>

      This websites uses the
      <a target="_blank" href="https://github.com/nickbabcock/jomini">Jomini</a>
      Javascript parser.
   </WelcomeItem>
</template>

<style scoped>
.download-anchor {
   display: flex;
   margin-right: auto;
   margin-left: auto;
   margin-bottom: 2rem;
}
.download-anchor:hover {
   background-color: transparent;
}
.download-json-file {
   cursor: pointer;
   background-color: hsla(160, 100%, 37%, 0.5);
   /* background-color: var(--color-background-mute); */
   outline: none;
   border-radius: 9999px;
   padding-left: 1.25rem; /* 20px */
   padding-right: 1.25rem; /* 20px */
   padding-top: 0.625rem; /* 10px */
   padding-bottom: 0.625rem; /* 10px */
   border: 0;
   color: var(--vt-c-text-dark-1);
   font-weight: 600;
}
.download-json-file:hover {
   background-color: hsla(160, 100%, 37%, 0.4);
}
.dropZone {
   /* width: 80%; */
   height: 200px;
   position: relative;
   border: 2px dashed #eee;
   margin: 1rem 0;
   /* margin: auto; */
}

.dropZone:hover {
   border: 2px solid #00bd7e;
}

.dropZone:hover .dropZone-title {
   color: #00bd7e;
}

.dropZone-info {
   color: #a8a8a8;
   position: absolute;
   top: 50%;
   width: 100%;
   transform: translate(0, -50%);
   text-align: center;
}

.dropZone-title {
   color: #787878;
}

.dropZone input,
.dropZone-uploaded input {
   position: absolute;
   cursor: pointer;
   top: 0px;
   right: 0;
   bottom: 0;
   left: 0;
   width: 100%;
   height: 100%;
   opacity: 0;
}

.dropZone-upload-limit-info {
   display: flex;
   justify-content: flex-start;
   flex-direction: column;
}

.dropZone-uploaded {
   height: 200px;
   position: relative;
   border: 2px dashed #eee;
   margin: 1rem 0;
   display: grid;
   place-content: center;
}

.new-file-upload {
   position: relative;
   margin-right: auto;
   margin-left: auto;
}
</style>
