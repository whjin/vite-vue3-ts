<template>
  <CPanel>
    <template #header>各行业收入</template>
    <template #content>
      <CEcharts ref="chartRef" :option @onload="startHighlightLoop" />
    </template>
  </CPanel>
</template>

<script setup lang="ts" name="IndustryRevenue">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as echarts from 'echarts'
import CPanel from '@/components/common/CPanel.vue'
import CEcharts from '@/components/common/CEcharts.vue'

const option = ref<any>({})
const chartRef = ref()
let highlightTimer: any = null
let currentIndex = 0
const VALUE = [100, 200, 300, 400, 500, 600, 700]

const createEchartBar = () => {
  const xAxisData = ['旅游', '住宿', '餐饮', '购物', '娱乐', '交通', '其他']
  const seriesData = [
    {
      value: 100,
    },
    {
      value: 200,
    },
    {
      value: 300,
    },
    {
      value: 400,
    },
    {
      value: 500,
    },
    {
      value: 600,
    },
    {
      value: 700,
    },
  ]

  let maxAmount = 0
  seriesData.map(item => {
    item.value > maxAmount ? (maxAmount = item.value) : (maxAmount = maxAmount)
  })

  return {
    grid: {
      left: '0%',
      right: '0%',
      top: '20%',
      bottom: '10%',
      containLabel: true,
    },
    xAxis: {
      type: 'category',
      data: xAxisData,
      axisLine: {
        show: true,
        lineStyle: {
          width: 2,
          color: 'rgba(76, 93, 130, 1)',
        },
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        fontSize: 12,
        color: '#C5D6E6',
        label: {
          textStyle: {
            color: '#C5D6E6',
          },
        },
      },
    },
    yAxis: {
      type: 'value',
      axisLine: {
        show: false,
      },
      name: '亿',
      nameTextStyle: {
        color: 'rgba(201, 211, 234, 1)',
        fontSize: 14,
        padding: [0, 32, 12, 0],
      },
      splitLine: {
        show: true,
        lineStyle: {
          color: 'rgba(52, 71, 112, 1)',
          type: 'dashed',
        },
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        fontSize: 14,
        color: '#C5D6E6',
        label: {
          textStyle: {
            color: '#C5D6E6',
            fontSize: 14,
          },
        },
      },
    },
    series: [
      {
        type: 'pictorialBar',
        name: '渐变背景',
        barWidth: 14,
        symbol: 'rect',
        symbolSize: '100%',
        symbolPosition: 'start',
        symbolOffset: [0, 0],
        label: {
          show: true,
          position: 'top',
          formatter: (params: any) => {
            return [...Object.values(seriesData[params.dataIndex])].join('\n')
          },
          fontSize: 12,
          lineHeight: 16,
          color: '#93B9FF',
        },
        emphasis: {
          itemStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgba(218, 163, 88, 1)',
              },
              {
                offset: 1,
                color: 'rgba(255, 130, 54, 1)',
              },
            ]),
          },
        },
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            {
              offset: 0,
              color: 'rgba(197, 213, 249, 1)',
            },
            {
              offset: 1,
              color: 'rgba(120, 144, 199, 1)',
            },
          ]),
        },
        z: 1,
        zlevel: 0,
        data: seriesData.map(item => item.value),
      },
      {
        type: 'pictorialBar',
        name: '块状切片',
        emphasis: {
          itemStyle: {
            color: '#011140',
          },
        },
        barWidth: 14,
        symbolRepeat: 28,
        symbol: 'rect',
        symbolClip: true,
        symbolSize: [14, 2],
        symbolPosition: 'start',
        symbolOffset: [0, 0],
        data: seriesData.map(item => item.value),
        z: 2,
        zlevel: 0,
      },
    ],
  }
}

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
onBeforeUnmount(() => {
  if (highlightTimer) clearInterval(highlightTimer)
})
</script>

<style lang="scss" scoped></style>
