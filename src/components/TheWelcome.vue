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

         console.log(this.fileName);
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
};
</script>

<template>
   <WelcomeItem>
      <template #icon>
         <DocumentationIcon />
      </template>
      <template #heading>Use</template>

      Just drag and drop a file below and download the json parsed file.

      <div>
         <input
            ref="doc"
            type="file"
            @change="onFileChanged($event)"
            accept=".txt"
            capture
         />
      </div>

      <a v-if="isFileReady" :href="link" :download="fileName">
         <button v-if="isFileReady" class="download-json-file">
            Your file is ready, click to download !
         </button>
      </a>
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
   <!-- 
  <WelcomeItem>
    <template #icon>
      <EcosystemIcon />
    </template>
    <template #heading>Ecosystem</template>

    Get official tools and libraries for your project:
    <a target="_blank" href="https://pinia.vuejs.org/">Pinia</a>,
    <a target="_blank" href="https://router.vuejs.org/">Vue Router</a>,
    <a target="_blank" href="https://test-utils.vuejs.org/">Vue Test Utils</a>, and
    <a target="_blank" href="https://github.com/vuejs/devtools">Vue Dev Tools</a>. If you need more
    resources, we suggest paying
    <a target="_blank" href="https://github.com/vuejs/awesome-vue">Awesome Vue</a>
    a visit.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <CommunityIcon />
    </template>
    <template #heading>Community</template>

    Got stuck? Ask your question on
    <a target="_blank" href="https://chat.vuejs.org">Vue Land</a>, our official Discord server, or
    <a target="_blank" href="https://stackoverflow.com/questions/tagged/vue.js">StackOverflow</a>.
    You should also subscribe to
    <a target="_blank" href="https://news.vuejs.org">our mailing list</a> and follow the official
    <a target="_blank" href="https://twitter.com/vuejs">@vuejs</a>
    twitter account for latest news in the Vue world.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>Support Vue</template>

    As an independent project, Vue relies on community backing for its sustainability. You can help
    us by
    <a target="_blank" href="https://vuejs.org/sponsor/">becoming a sponsor</a>.
  </WelcomeItem> -->
</template>

<style scoped>
.download-json-file {
   cursor: pointer;
}
</style>
