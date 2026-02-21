<template>
  <div id="map" class="map">
    <div class="map-content">
      <CEcharts :option="mapOption" />
    </div>
  </div>
</template>

<script setup lang="ts" name="CMap">
import { ref } from 'vue'
import { getMapOption } from '@/modules/echartMap'
import CEcharts from './common/CEcharts.vue'

const mapOption = ref(getMapOption())
</script>

<style lang="scss" scoped>
.map {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .map-content {
    position: relative;
    width: 1920px;
    height: 1080px;
  }
}
</style>
