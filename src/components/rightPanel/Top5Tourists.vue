<template>
  <CPanel>
    <template #header>接待游客人数TOP5</template>
    <template #content>
      <CEcharts ref="chartRef" :option @onload="startHighlightLoop" />
    </template>
  </CPanel>
</template>

<script setup lang="ts" name="Top5Tourists">
import { ref, onMounted, onUnmounted } from 'vue'
import * as echarts from 'echarts'
import CPanel from '@/components/common/CPanel.vue'
import CEcharts from '@/components/common/CEcharts.vue'

const option = ref<any>({})
const chartRef = ref()
let highlightTimer: any = null
let currentIndex = 0
const VALUE = [123, 100, 125, 100, 125]

const createEchartBar = () => {
  return {
    /**区域位置*/
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true,
    },
    tooltip: {
      trigger: 'axis',
    },
    xAxis: {
      type: 'category',
      data: ['青岛市', '济南市', '烟台市', '威海市', '潍坊市'],
      axisLine: {
        show: false,
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        color: '#C5D6E6',
        fontSize: 12,
      },
    },
    yAxis: {
      axisLine: {
        show: false,
      },
      splitLine: {
        lineStyle: {
          color: 'rgba(52, 71, 112, 1)',
          type: 'dashed',
        },
      },
      axisLabel: {
        color: '#C5D6E6',
        fontSize: 14,
      },
      axisTick: {
        show: false,
      },
    },
    series: [
      {
        name: '数(人次)',
        type: 'pictorialBar',
        barWidth: '150%',
        symbol: 'path://M0,10 L10,10 C5.5,10 5.5,5 5,0 C4.5,5 4.5,10 0,10 z',
        label: {
          show: true,
          position: 'top',
          fontSize: 14,
          color: 'rgba(201, 211, 234, 1)',
          offset: [0, -10],
        },
        itemStyle: {
          normal: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(
              0,
              1,
              0,
              0,
              [
                {
                  offset: 0,
                  color: 'rgba(94, 111, 153, 0.2)', // 0% 处的颜色
                },
                {
                  offset: 0.6,
                  color: 'rgba(94, 111, 153, 0.8)', // 60% 处的颜色
                },
                {
                  offset: 1,
                  color: 'rgba(94, 111, 153, 1)', // 100% 处的颜色
                },
              ],
              false,
            ),
            borderColor: 'rgba(109, 128, 175, 1)', // 边框颜色
            borderWidth: 4, // 边框宽度
            borderRadius: [100, 100],
          },
          emphasis: {
            color: new echarts.graphic.LinearGradient(
              0,
              1,
              0,
              0,
              [
                {
                  offset: 0,
                  color: 'rgba(218, 163, 88, 0.3)', // 0% 处的颜色
                },
                {
                  offset: 0.6,
                  color: 'rgba(218, 163, 88, 0.8)', // 60% 处的颜色
                },
                {
                  offset: 1,
                  color: 'rgba(218, 163, 88, 0)', // 100% 处的颜色
                },
              ],
              false,
            ),
            borderColor: 'rgba(218, 163, 88, 1)', // 边框颜色
          },
        },
        data: [123, 100, 125, 100, 125],
        z: 10,
      },
    ],
  }
}

// 高亮循环方法
const startHighlightLoop = (chart: any) => {
  if (!chart) return

  // 如果已经存在定时器，先清除
  if (highlightTimer) {
    clearInterval(highlightTimer)
    highlightTimer = null
  }

  highlightTimer = setInterval(() => {
    // 取消之前的高亮
    chart.dispatchAction({
      type: 'downplay',
    })
    // 高亮当前柱子
    chart.dispatchAction({
      type: 'highlight',
      seriesIndex: 0,
      dataIndex: currentIndex,
    })
    // 更新索引，循环
    currentIndex = (currentIndex + 1) % VALUE.length
  }, 1500)
}

onMounted(() => {
  option.value = createEchartBar()
})
onUnmounted(() => {
  if (highlightTimer) {
    clearInterval(highlightTimer)
  }
})
</script>
<style lang="scss" scoped></style>
