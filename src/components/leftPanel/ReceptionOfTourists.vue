<template>
  <CPanel>
    <template #header>年度接待游客比</template>
    <template #content>
      <CEcharts :option />
    </template>
  </CPanel>
</template>

<script setup lang="ts" name="ReceptionOfTourists">
import { ref, onMounted } from 'vue'
import * as echarts from 'echarts'
import CPanel from '@/components/common/CPanel.vue'
import CEcharts from '@/components/common/CEcharts.vue'

const option = ref<any>({})
const createEchartLine = () => {
  return {
    tooltip: {
      trigger: 'axis',
    },
    legend: {
      top: '5%',
      right: '2%',
      itemGap: 20,
      itemWidth: 15,
      itemHeight: 1,
      textStyle: {
        color: '#C5D6E6',
      },
    },
    grid: {
      left: '1%',
      right: '2%',
      bottom: '10%',
      containLabel: true,
    },
    xAxis: [
      {
        type: 'category',
        data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
        axisLabel: {
          textStyle: {
            color: '#C5D6E6',
          },
        },
        axisLine: {
          lineStyle: {
            color: 'rgba(76, 93, 130, 1)',
          },
        },
        axisTick: {
          show: false,
        },
      },
    ],
    yAxis: [
      {
        type: 'value',
        name: '万人',
        nameTextStyle: {
          color: 'rgba(201, 211, 234, 1)',
          fontSize: 14,
          padding: [0, 32, 12, 0],
        },
        splitNumber: 4,
        splitLine: {
          lineStyle: {
            color: 'rgba(52, 71, 112, 1)',
            type: 'dashed',
          },
        },
        axisLabel: {
          textStyle: {
            color: '#C5D6E6',
            fontSize: 14,
          },
        },
        axisLine: {
          show: false,
        },
      },
    ],
    series: [
      {
        name: '2021年',
        type: 'line',
        data: [23, 60, 20, 36, 23, 85, 70, 60, 78, 89, 68, 56],
        lineStyle: {
          normal: {
            width: 2,
            color: 'rgba(218, 163, 88, 1)',
            shadowColor: 'rgba(218, 163, 88, 0.3)',
            shadowBlur: 10,
            shadowOffsetY: 20,
          },
        },
        areaStyle: {
          normal: {
            color: new echarts.graphic.LinearGradient(
              0,
              0,
              0,
              1,
              [
                {
                  offset: 0,
                  color: 'rgba(218, 163, 88, 1)',
                },
                {
                  offset: 1,
                  color: 'rgba(218, 163, 88, 0)',
                },
              ],
              false,
            ),
          },
        },
        itemStyle: {
          color: 'rgba(15, 222, 255, 1)',
        },
        smooth: true,
        symbol: 'none',
      },
      {
        name: '2022年',
        type: 'line',
        data: [145, 78, 88, 99, 36, 109, 120, 150, 99, 89, 100, 120],
        lineStyle: {
          normal: {
            width: 2,
            color: 'rgba(109, 128, 175, 1)',
          },
        },
        areaStyle: {
          normal: {
            color: new echarts.graphic.LinearGradient(
              0,
              0,
              0,
              1,
              [
                {
                  offset: 0,
                  color: 'rgba(109, 128, 175, 1)',
                },
                {
                  offset: 1,
                  color: 'rgba(109, 128, 175, 0)',
                },
              ],
              false,
            ),
          },
        },
        itemStyle: {
          color: 'rgba(109, 128, 175, 1)',
        },
        smooth: true,
        symbol: 'none',
      },
    ],
  }
}

onMounted(() => {
  option.value = createEchartLine()
})
</script>

<style lang="scss" scoped></style>
