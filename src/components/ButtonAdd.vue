<template>
  <div class="btn-add disable-select">+</div>
</template>
<script setup></script>
<style scoped>
.btn-add {
  height: 80px;
  width: 80px;
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);

  background: var(--ui-blue);
  line-height: 80px;
  text-align: center;
  font-size: 30px;
  font-weight: 700;
  border-radius: 100%;
  box-shadow: var(--ui-shadow);
  color: rgb(223, 223, 223);
}

.btn-add:hover {
  cursor: pointer;
  background: var(--ui-blue-hover);
}
</style>
