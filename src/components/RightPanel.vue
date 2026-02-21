<template>
  <div class="right-panel">
    <IndustryRevenue />
    <Top5Tourists />
    <RealTimeHotWords />
  </div>
</template>

<script setup lang="ts" name="RightPanel">
import IndustryRevenue from './rightPanel/IndustryRevenue.vue'
import Top5Tourists from './rightPanel/Top5Tourists.vue'
import RealTimeHotWords from './rightPanel/RealTimeHotWords.vue'
</script>

<style lang="scss" scoped>
.right-panel {
  position: absolute;
  width: 500px;
  height: 100%;
  right: 0;
  top: 0;
  display: grid;
  gap: 24px;
  padding: 111px 0 24px 0;
  box-sizing: border-box;
  grid-template-columns: 1fr;
  grid-template-rows: repeat(3, 1fr);
}
.panel {
  right: -500px;
  animation: entranceAnimation ease-in-out 0.75s forwards;
}
@keyframes entranceAnimation {
  0% {
    right: -500px;
  }
  100% {
    right: 0;
  }
}
</style>
