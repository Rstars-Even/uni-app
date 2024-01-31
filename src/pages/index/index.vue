<template>
  <!-- 自定义导航栏组件 -->
  <CustomNavbar />
  <!-- 轮播图组件 -->
  <XtxSwiper :list="bannerList" />
  <!-- 轮播图组件 -->
  <CategoryPanel :list="categoryList" />
  <view class="index"> </view>
</template>

<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import CategoryPanel from './components/CategoryPanel.vue'
import { getHomeBannerAPI, getHomeCategoryAPI } from '@/services/home'
import type { BannerItem, CategoryItem } from '@/types/home'
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

onLoad(() => {
  getHomeBannerData()
  categoryListrData()
})
</script>

<style lang="scss">
page {
  background-color: #f7f7f7;
}
</style>
