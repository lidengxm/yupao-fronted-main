<template>
  <van-skeleton title avatar :row="3" :loading="props.loading" v-for="user in props.userList">
    <van-swipe-cell>
      <van-card v-if="user"
          :desc="user.profile ?'个人简介：'+ user.profile:'个人简介：该用户未设置'"
          :thumb="user.avatarUrl ? user.avatarUrl :defaultPicture "
          :title="user.username"
          @click="showUser(user?.id)"
      >
        <template #tags>
          <div v-if="user.tags.length<7" style="margin-bottom: 12px"></div>
          标签：<br>
          <van-tag v-for="tag in user.tags" style="color: rgb(245, 67, 67)" class="tag" plain type="primary">
            {{ tag }}
          </van-tag>
          <span v-if="!user.tags||user.tags.length<=0">该用户暂时没有设置</span>
        </template>
      </van-card>
    </van-swipe-cell>
    <div style="padding-top: 5px"></div>
  </van-skeleton>
</template>

<script setup lang="ts">
import {UserType} from "../models/user";
import {useRouter} from "vue-router";

const router = useRouter()
const defaultPicture = "https://img.qimuu.icu/typory/teamImg1.jpg"

interface UserCardListProps {
  loading: boolean;
  userList: UserType[];
}

//得到父组件传来的参数
const props = withDefaults(defineProps<UserCardListProps>(), {
  loading: true,
  // @ts-ignore
  userList: [] as UserType[],
});

/**
 * 查看用户详细信息
 * @param id 
 */
//路由配置
const showUser = (id:number) => {
    router.push({
        path: "/user/get",
        query: {
            id: id
        }
    })
}

</script>

<style scoped>
@import "../assets/css/public.css";

:deep(.van-search__field) {
  flex: 1;
  align-items: center;
  padding: 0 var(--van-padding-xs) 0 0;
  height: var(--van-search-input-height);
  background-color: transparent;
}

</style>
