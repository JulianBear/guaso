<template>
  <a-list item-layout="horizontal" :data-source="postList">
    <template #renderItem="{ item }">
      <a-list-item>
        <a-list-item-meta
          :description="item.content"
        >
          <template #title>
            <a href="https://www.antdv.com/">{{ item.title }}</a>
          </template>
          <template #avatar>
            <a-avatar :src="gasket" />
          </template>
        </a-list-item-meta>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import gasket from "../assets/awas.png";
import { withDefaults,defineProps } from "vue";
interface Props{
postList:any[]
}
const props=
  withDefaults(defineProps<Props>(),{
    postList:()=>[]
  }
)
</script>
<style scoped>
.gas {
  width: 200px;
}
</style>