<template>
  <FileUpload v-if="!fileSelected" @fileUploaded="handleFileUpload" />
  <TheTable v-if="fileSelected" :head="tableHead" :content="tableContent" />
  <ButtonAdd v-if="fileSelected" @click="handleAddRow" />
  <FileExport v-if="fileSelected" :cellsPerRow="tableHead.length" />
</template>

<script setup>
import FileUpload from "@/components/FileUpload.vue";
import TheTable from "@/components/TheTable.vue";
import FileExport from "@/components/FileExport.vue";
import ButtonAdd from "@/components/ButtonAdd.vue";
import { ref } from "vue";

const tableHead = ref([]);
const tableContent = ref([]);
const fileSelected = ref(false);

const handleFileUpload = (content) => {
  tableHead.value = content[0];
  content.splice(0, 1);
  tableContent.value = content;
  fileSelected.value = true;
};

const handleAddRow = () => {
  let cellsPerRow = tableHead.value.length;
  let newRow = [];
  for (let i = 0; i < cellsPerRow; i++) {
    newRow.push("");
  }
  tableContent.value.push(newRow);
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.disable-select {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>
