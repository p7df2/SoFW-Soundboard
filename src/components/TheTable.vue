<template>
  <div class="table">
    <div class="row header">
      <div class="cell">1</div>
      <div
        class="cell table-content"
        v-for="colHead in props.head"
        :key="colHead"
        contenteditable="true"
      >
        {{ colHead }}
      </div>
      <div class="cell"></div>
    </div>

    <div class="row" v-for="(row, index) in props.content" :key="index">
      <div class="cell index">{{ index + 2 }}</div>
      <div
        class="cell table-content"
        v-for="val in row"
        :key="val"
        contenteditable="true"
      >
        {{ val }}
      </div>
      <div class="cell btn-remove" @click="toggleRemove(index)"></div>
    </div>
  </div>
</template>
<script setup>
import { defineProps } from "vue";
const props = defineProps(["head", "content"]);

const toggleRemove = (rowNumber) => {
  let rows = document.getElementsByClassName("row");

  if (rows[rowNumber + 1].classList.contains("removed")) {
    rows[rowNumber + 1].classList.remove("removed");
    let cells = rows[rowNumber + 1].children;
    for (let i = 0; i < cells.length; i++) {
      if (
        !cells[i].classList.contains("index") &&
        !cells[i].classList.contains("btn-remove")
      ) {
        cells[i].classList.add("table-content");
      }
    }
  } else {
    rows[rowNumber + 1].classList.add("removed");
    let cells = rows[rowNumber + 1].children;
    for (let i = 0; i < cells.length; i++) {
      cells[i].classList.remove("table-content");
    }
  }
};
</script>
<style>
.table {
  width: 100%;
  display: table;
  padding-bottom: 150px;
  background: white;
  position: relative;
}
@media screen and (max-width: 768px) {
  .table {
    width: 100vw;
  }
}
.row {
  display: table-row;
  background: #fff;
}
.row.header {
  background: var(--ui-blue);
  position: -webkit-sticky;
  position: sticky;
  top: 0;
}

.cell {
  display: table-cell;
  text-align: center;
  border: solid rgb(250, 249, 249) 1px;
}

.row .cell {
  font-size: 15px;
  color: #666;
  line-height: 1.2;
  font-weight: unset !important;
  padding-top: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #f2f2f2;
}
@media screen and (max-width: 768px) {
  .row .cell {
    font-size: 13px;
    line-height: 1;
  }
}

.row.header .cell {
  font-size: 18px;
  color: #fff;
  line-height: 1.2;
  font-weight: unset !important;
  border: solid #0fa4af 1px;
}
@media screen and (max-width: 768px) {
  .row.header .cell {
    font-size: 15px;
    line-height: 1;
  }
}

.cell:hover {
  background-color: #ececff;
  cursor: pointer;
}

.header .cell:hover {
  background-color: rgb(79, 78, 78);
}

.row.removed .cell {
  background: rgb(193, 177, 177);
  text-decoration: line-through;
}
.row.removed .cell {
  border: solid rgb(209, 198, 198) 1px;
}

.row .btn-remove {
  font-size: 20px;
}
@media screen and (max-width: 768px) {
  .row .btn-remove {
    font-size: 15px;
  }
}

.row .btn-remove:hover {
  opacity: 0.8;
}
.row.removed .btn-remove {
  text-decoration: none;
}

.row .btn-remove:before {
  content: "❌";
}

.row.removed .btn-remove:before {
  content: "🔄";
}
</style>
