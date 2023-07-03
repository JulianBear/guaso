<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchText"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />

  <MyDivider></MyDivider>
  <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
    <a-tab-pane key="Post" tab="POST"><PostList :post-list="postList"/></a-tab-pane>
    <a-tab-pane key="User" tab="USER" > <UserList :user-list="userList"/></a-tab-pane>
    <a-tab-pane key="Picture"  tab="PICTURE"> <PictureList :picture-list="pictureList"/></a-tab-pane>
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
import { message } from "ant-design-vue";
const postList=ref([]);
const userList=ref([]);
const pictureList=ref([]);

const router = useRouter();
const route =useRoute();
const activeKey = route.params.category;
const searchText = ref(route.query.text || "");
const initSearchParams ={
  text:'',
  pageSize:10,
  pageNum:1,
  type:route.params.category
}


const searchParams = ref(
 initSearchParams
)

const loadAllData = (params:any)=>{
  const query = {
    ...params,
    searchText: params.text,
  };

myAxios.post("search/crawSearchAll",query).then((res:any)=>{
 postList.value=res.dataList;
 userList.value=res.dataList;
 pictureList.value=res.dataList;

 });
}
const loadData = (params:any)=>{
  
  
  const { type } = params;
  const query = {
    ...params,
    searchText: params.text,
  };
if(!type){
  message.error('类别为空0.0')
  return;
}
myAxios.post("search/crawSearchFacade",query).then((res:any)=>{
if(type=="Post"){
  postList.value=res.dataList;
 console.log(postList)
}
else if(type=="User"){
  userList.value=res.dataList;
  console.log(userList)
}
else if(type == "Picture"){
  pictureList.value=res.dataList;
  console.log(pictureList)
  
}
 });
}

//loadAllData(initSearchParams);
//TODO页面重新刷新 props组件为null
watchEffect(()=>{
searchParams.value={
  ...initSearchParams,
  text:route.query.text,
  type:route.params.category
} as any;
loadData(searchParams.value);
});

const onSearch = (value: string) => {
  router.push({
    query: {
      ...searchParams.value,
      text: value,
    },
  });
 

};
const onTabChange=(key:string)=>{
  router.push({
    path:`/${key}`,
    query:searchParams.value
  })
}
</script>
