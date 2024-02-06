<template>
  <!-- 自定义导航栏组件 -->
  <CustomNavbar />
  <scroll-view
    refresher-enabled
    @refresherrefresh="onRefresherrefresh"
    :refresher-triggered="isTriggered"
    scroll-y
    @scrolltolower="onScrolltolower"
    class="scroll-view"
  >
    <!-- 骨架屏组件。 -->
    <PageSkeleton v-if="isLoading" />
    <template v-else>
      <!-- 轮播图组件 -->
      <XtxSwiper :list="bannerList" />
      <!-- 首页分类组件 -->
      <CategoryPanel :list="categoryList" />
      <!-- 热门推荐组件 -->
      <HotPanel :list="hotList" />
      <!-- 猜你喜欢组件 -->
      <XtxGuess ref="guessRef" />
    </template>
  </scroll-view>
</template>

<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import CategoryPanel from './components/CategoryPanel.vue'
import HotPanel from './components/HotPanel.vue'
import PageSkeleton from './components/PageSkeleton.vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import type { BannerItem, CategoryItem, HotItem } from '@/types/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'

// 获取猜你喜欢组件实例。
const guessRef = ref()
// 滚动触底事件。
const onScrolltolower = () => {
  guessRef.value?.getMore()
}

// 当前下拉刷新状态。
const isTriggered = ref(false)
// 下拉刷新事件。
const onRefresherrefresh = async () => {
  // 开始动画
  isTriggered.value = true
  guessRef.value?.resetData()
  // 下拉首页重新加载数据。
  // await getHomeBannerData()
  // await categoryListrData()
  // await hotListrData()
  // await guessRef.value?.getMore()
  // 优化。
  await Promise.all([
    getHomeBannerData(),
    categoryListrData(),
    hotListrData(),
    guessRef.value?.getMore(),
  ])
  // 结束动画
  isTriggered.value = false
}

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

const isLoading = ref(false)

onLoad(async () => {
  isLoading.value = true
  await Promise.all([getHomeBannerData(), categoryListrData(), hotListrData()])
  isLoading.value = false
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
