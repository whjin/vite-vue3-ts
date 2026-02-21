<template>
  <div class="left-panel">
    <RankingOfScenicSpots />
    <AgeDistribution />
    <ReceptionOfTourists />
  </div>
</template>

<script setup name="LeftPanel">
import RankingOfScenicSpots from './leftPanel/RankingOfScenicSpots.vue'
import AgeDistribution from './leftPanel/AgeDistribution.vue'
import ReceptionOfTourists from './leftPanel/ReceptionOfTourists.vue'
</script>

<style lang="scss" scoped>
.left-panel {
  position: absolute;
  width: 500px;
  height: 100%;
  left: 0;
  top: 0;
  display: grid;
  gap: 24px;
  padding: 111px 0 34px 16px;
  box-sizing: border-box;
  grid-template-columns: 1fr;
  grid-template-rows: repeat(3, 1fr);
}
.panel {
  left: -500px;
  animation: entranceAnimation ease-in-out 0.75s forwards;
}
@keyframes entranceAnimation {
  0% {
    left: -500px;
  }
  100% {
    left: 0;
  }
}
</style>
