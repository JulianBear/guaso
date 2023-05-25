<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />

  <MyDivider></MyDivider>
  <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
    <a-tab-pane key="Post" tab="Tab 1">
       <PostList :post-list="postList"/>
      </a-tab-pane>
    <a-tab-pane key="User" tab="Tab 2" > <UserList :user-list="userList"/></a-tab-pane>
    <a-tab-pane key="Picture" tab="Tab 3"> <PictureList /></a-tab-pane>
  </a-tabs>
  </div>
</template>
<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/UserList.vue";
import PictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";
import {  useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";
const postList=ref([]);
const userList=ref([]);

myAxios.post("post/list/page/vo",{}).then((res:any)=>{
  console.log(res)
postList.value=res.records
});

myAxios.post("user/list/page/vo",{}).then((res:any)=>{
  console.log(res)
  userList.value=res.records
});

const initSearchParams ={
  text:'',
  pageSize:10,
  pageNum:1
}
const searchParams = ref(
 initSearchParams
)
const router = useRouter();
const route =useRoute();
const activeKey = route.params.category;

watchEffect(()=>{
searchParams.value={
  ...initSearchParams,
  text:route.query.text 
} as any;
});
const onSearch = (value: string) => {
  alert(value);

  router.push({
    query:searchParams.value
  })
};
const onTabChange=(key:string)=>{
  router.push({
    path:`/${key}`,
    query:searchParams.value
  })
}
</script>
