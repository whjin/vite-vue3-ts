<template>
  <div class="echarts" ref="echartsRef"></div>
</template>

<script setup lang="ts" name="CEcharts">
import { ref, reactive, onMounted, watch, onBeforeMount } from 'vue'
import * as echarts from 'echarts'

const emit = defineEmits(['eclick', 'onload'])

const prop = withDefaults(defineProps<{ option: any }>(), {
  option: () => {
    return {}
  },
})

const echartsRef = ref()
const state = reactive({
  option: null,
})

let echartsInstance: echarts.ECharts | null = null

watch(
  () => prop.option,
  () => {
    state.option = prop.option
    if (state.option) {
      echartsInstance?.setOption(state.option)
      emit('onload', echartsInstance)
    }
  },
  { immediate: false, deep: true },
)

const echartsResize = () => {
  echartsInstance?.resize()
}

function setDataZoom(start: any, end: any) {
  ;(state.option as any).dataZoom[0].start = start
  ;(state.option as any).dataZoom[0].end = end
  echartsInstance?.setOption(state.option as any)
}

const Eaclick = (val: any) => {
  emit('eclick', val)
}

onMounted(() => {
  state.option = prop.option
  if (echartsInstance) {
    if (state.option) {
      echartsInstance?.setOption(state.option)
    }
  } else {
    echartsInstance = echarts.init(echartsRef.value as HTMLDivElement)
    echartsInstance.on('click', function (val) {
      Eaclick(val)
    })
    if (state.option) {
      echartsInstance?.setOption(state.option)
    }
  }
  window.addEventListener('resize', echartsResize)
})

onBeforeMount(() => {
  if (!echartsInstance) return
  window.removeEventListener('resize', echartsResize)
  echartsInstance.dispose()
})

defineExpose({ setDataZoom })
</script>

<style lang="scss" scoped>
.echarts {
  position: relative;
  width: 100%;
  height: 100%;
  margin: auto;
}
</style>
