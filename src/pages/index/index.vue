<template>
  <!-- 自定义导航栏组件 -->
  <CustomNavbar />
  <scroll-view scroll-y class="scroll-view">
    <!-- 轮播图组件 -->
    <XtxSwiper :list="bannerList" />
    <!-- 首页分类组件 -->
    <CategoryPanel :list="categoryList" />
    <!-- 热门推荐组件 -->
    <HotPanel :list="hotList" />
    <!-- 猜你喜欢组件 -->
    <XtxGuess />
  </scroll-view>
</template>

<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import CategoryPanel from './components/CategoryPanel.vue'
import HotPanel from './components/HotPanel.vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import type { BannerItem, CategoryItem, HotItem } from '@/types/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'

// 获取轮播图数据。。
const bannerList = ref<BannerItem[]>([])
const getHomeBannerData = async () => {
  const res = await getHomeBannerAPI()
  // console.log('----', res);
  bannerList.value = res.result
}
// 首页分类数据。。
const categoryList = ref<CategoryItem[]>([])
const categoryListrData = async () => {
  const res = await getHomeCategoryAPI()
  // console.log('----', res);
  categoryList.value = res.result
}
// 热门推荐数据。。
const hotList = ref<HotItem[]>([])
const hotListrData = async () => {
  const res = await getHomeHotAPI()
  // console.log('----', res);
  hotList.value = res.result
}

onLoad(() => {
  getHomeBannerData()
  categoryListrData()
  hotListrData()
})
</script>

<style lang="scss">
page {
  background-color: #f7f7f7;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.scroll-view {
  flex: 1;
}
</style>
