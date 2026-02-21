<template>
  <main class="container">
    <CHeader />
    <CMap />
    <LeftPanel />
    <RightPanel />
    <CFooter />
  </main>
</template>

<script setup name="index">
import { onMounted } from 'vue'
import CHeader from '@/components/CHeader.vue'
import CMap from '@/components/CMap.vue'
import LeftPanel from '@/components/LeftPanel.vue'
import RightPanel from '@/components/RightPanel.vue'
import CFooter from '@/components/CFooter.vue'
import autofit from 'autofit.js'

onMounted(() => {
  autofit.init({
    el: 'body',
    dw: 1920,
    dh: 1080,
    resize: true,
  })
})
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  width: 100%;
  height: 100%;
  background: url('@/assets/images/背景.png') no-repeat center center;
  background-size: 100% 100%;
}
</style>
