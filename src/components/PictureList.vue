<template>
  图片列表
  <img :src="gasket" class="gas"/>
</template>

<script setup lang="ts">
import gasket from "../assets/keyBoard.jpg"
</script>
<style scoped>
.gas {
  width: 200px;
}
</style>