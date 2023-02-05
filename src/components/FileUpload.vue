<template>
  <div id="upload-container">
    <button id="btn-upload" class="disable-select" @click="onPickFile">Select File</button>
    <input
      id="fileInput"
      type="file"
      style="display: none"
      accept=".csv"
      @change="onFilePicked"
    />
  </div>
</template>
<script setup>
import { defineEmits } from "vue";
const emit = defineEmits(["fileUploaded"]);

const onPickFile = () => {
  document.getElementById("fileInput").click();
};

const onFilePicked = (event) => {
  if (event.target.files.length === 1) {
    let uploadedFile = event.target.files[0];
    if (validateFileType(uploadedFile.name)) {
      fileToTableRowArr(uploadedFile);
    }
  } else {
    alert("Please upload a single file only");
  }
};

const validateFileType = (el) => {
  let regex = new RegExp("(.*?)(csv)$");
  if (!regex.test(el.toLowerCase())) {
    el = "";
    alert("The file format is not supported");
    return false;
  }
  return true;
};

const fileToTableRowArr = async (file) => {
  const fileContent = await file.text();

  //An array where each index contains the row content as a string
  let rows = fileContent.split(/\r?\n|\r|\n/g);

  if (rows.length < 1) {
    alert("The uploaded file does not contain any data");
    return;
  }
  let delimiter = detectDelimiter(rows);

  //An array where each index contains an array with the table values of the row
  let tableRowsArr = [];
  rows.forEach((row) => {
    let nextRow = row.split(delimiter);

    //Fills up any whitespace cells
    if (tableRowsArr.length > 1 && nextRow.length < tableRowsArr[0].length) {
      for (let i = 1; i < tableRowsArr[0].length; i++) {
        nextRow.push("");
      }
    }
    tableRowsArr.push(nextRow);
  });

  emit("fileUploaded", tableRowsArr);
};

const detectDelimiter = (rows) => {
  let possibleDelimiters = [",", ";", "|", "    "];
  let delimiter = ""; //empty string to avoid failure if there is only 1 col for each row (which means no delimiter)

  //if both arrays are equal in length && the length is greater than 1 the delimiter was found
  possibleDelimiters.forEach((del) => {
    let colCountRow1 = rows[0].split(del).length;
    let colCountRow2 = rows[1].split(del).length;
    if (
      colCountRow1 === colCountRow2 &&
      colCountRow1 !== 1 &&
      colCountRow2 !== 1
    ) {
      delimiter = del;
    }
  });

  return delimiter;
};
</script>

<style scoped>
#upload-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

#btn-upload {
  background: none;
  height: 180px;
  width: 180px;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 1px;
  color: white;
  border: dotted white 4px;
  box-shadow: var(--ui-shadow);
}

#btn-upload:hover {
  border: solid white 4px;
  cursor: pointer;
}
</style>
