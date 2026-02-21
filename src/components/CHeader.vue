<template>
  <header class="header">山东省旅游指标监控平台</header>
  <div class="text-carousel">
    <transition-group name="carousel" tag="div" class="carousel-container">
      <div class="text-carousel-item" v-show="currentIndex === getCurrentItemIndex()" :key="currentIndex">
        {{ getCurrentItem() }}
      </div>
    </transition-group>
  </div>
</template>

<script setup lang="ts" name="CHeader">
import { ref, onMounted, onBeforeUnmount } from 'vue'

const carouselData = ref([
  '济宁市微山湖旅游区达到国家5A级旅游景区标准要求，拟确定为国家5A级旅游景区!',
  '山东省文化和旅游厅发布《关于促进文化和旅游消费的若干措施》',
  '青岛市崂山风景区入选"中国最美旅游景区"榜单',
  '烟台市蓬莱阁景区推出夜游项目，游客体验度大幅提升',
  '威海市刘公岛景区新增海洋文化展览馆',
  '日照市万平口海滨风景区完成升级改造',
  '泰安市泰山景区推出智慧旅游服务系统',
  '临沂市蒙山景区举办"蒙山红叶节"活动',
  '淄博市周村古商城景区新增非遗文化展示区',
  '潍坊市青州古城景区推出沉浸式文化体验项目',
  '东营市黄河口生态旅游区举办观鸟节活动',
  '滨州市孙子兵法城景区新增军事文化体验项目',
  '德州市齐河欧乐堡景区推出亲子游主题活动',
  '聊城市东昌湖景区举办荷花节活动',
  '菏泽市牡丹园景区新增夜间灯光秀表演',
  '枣庄市台儿庄古城景区推出运河文化体验游',
  '济宁市曲阜三孔景区举办传统文化研学活动',
])

const currentIndex = ref(0)
const timer = ref<number | null>(null)
const isAnimating = ref(false)
const isPaused = ref(false)

const config = {
  interval: 5000,
  animation_duration: 500,
  debounce_delay: 100,
}

const getCurrentItem = (): string => {
  if (carouselData.value.length === 0) return ''

  return carouselData.value[currentIndex.value]
}

const getCurrentItemIndex = (): number => {
  return currentIndex.value
}

const nextItem = (): void => {
  if (isAnimating.value || isPaused.value) return

  try {
    isAnimating.value = true
    setTimeout(() => {
      currentIndex.value = (currentIndex.value + 1) % carouselData.value.length
      isAnimating.value = false
    }, config.animation_duration)
  } catch (err) {
    console.error('轮播切换出错:', err)
    isAnimating.value = false
  }
}

const startCarousel = (): void => {
  if (timer.value) clearInterval(timer.value)
  timer.value = setInterval(() => {
    nextItem()
  }, config.interval)
}

onMounted(() => {
  startCarousel()
})

onBeforeUnmount(() => {
  if (timer.value) {
    clearInterval(timer.value)
    timer.value = null
  }
})
</script>

<style lang="scss" scoped>
.header {
  position: absolute;
  margin: 0 12px;
  top: 12px;
  width: calc(100% - 24px);
  height: 87px;
  background: url('@/assets/images/顶部标题.png') no-repeat center center;
  background-size: 100% 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  font-weight: 800;
  letter-spacing: 12px;
  color: rgba(230, 239, 253);
}

.text-carousel {
  position: absolute;
  top: 120px;
  left: 50%;
  width: 744px;
  height: 43px;
  transform: translateX(-50%);
  background: linear-gradient(90deg, rgba(218, 163, 88, 0), rgba(218, 163, 88, 0.6), rgba(218, 163, 88, 0));
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  overflow: hidden;
  cursor: pointer;
  pointer-events: all;
  &:hover {
    background: linear-gradient(90deg, rgba(218, 163, 88, 0.1), rgba(218, 163, 88, 0.7), rgba(218, 163, 88, 0.1));
  }
}

.carousel-container {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.text-carousel-item {
  position: absolute;
  width: 100%;
  text-align: center;
  padding: 0 20px;
  box-sizing: border-box;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  transform: translateY(0);
  opacity: 1;
}

.carousel-enter-active,
.carousel-leave-active {
  transition: all 0.5s ease-in-out;
}

.carousel-enter-from {
  transform: translateY(100%);
  opacity: 0;
}

.carousel-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
