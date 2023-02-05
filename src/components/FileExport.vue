<template>
  <div class="btn-export disable-select" @click="exportFile">Export File</div>
</template>
<script setup>
import { defineProps } from "vue";
const props = defineProps(["cellsPerRow"]);

const exportFile = () => {
  let fileContent = "";
  let cells = document.getElementsByClassName("table-content");

  for (let i = 0; i < cells.length; i++) {
    //Adds linebreak at the end of each row
    if (i !== 0 && i % props.cellsPerRow === 0) {
      fileContent = fileContent + "\n";
    }
    //Appends the cell content to the output string
    if (cells[i].innerText !== "") {
      //Removes possible <br> tags from cell content
      let containedTags = cells[i].children;
      for (let j = 0; j < containedTags.length; j++) {
        if (containedTags[j].tagName == "BR") {
          containedTags[j].remove();
        }
      }

      let cellContent = cells[i].innerText;
      //Removes any possible linebreaks from cell content
      cellContent.replace(/(\r\n|\n|\r)/gm, "");
      fileContent = fileContent + cellContent;
    }
    //Adds delimiter if it's not the last cell of the row
    if (i % props.cellsPerRow !== props.cellsPerRow - 1) {
      fileContent = fileContent + ";";
    }
  }

  let a = document.createElement("a");
  a.href = "data:attachment/csv," + fileContent;
  a.download = "export.csv";
  a.click();

  console.log(fileContent);
};
</script>
<style scoped>
.btn-export {
  height: 100px;
  width: 100px;
  border-radius: 20px;
  color: white;
  font-size: 18px;
  text-align: center;
  line-height: 100px;
  position: fixed;
  right: 20px;
  bottom: 20px;
  background: var(--ui-blue);
  box-shadow: var(--ui-shadow);
}

.btn-export:hover {
  cursor: pointer;
  background: var(--ui-blue-hover);
}
</style>
