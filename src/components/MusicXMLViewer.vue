<template>
  <div class="w-full h-full">
    <svg ref="svg" class="w-full h-full"></svg>
  </div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import initApp from './MusicXMLViewer.js'

// 可选：把你的 jianpu 渲染函数以参数形式传入
// 例如：import { jianpu } from './jianpu-renderer.js'

const svg = ref(null)
let cleanup = null

onMounted(() => {
  if (!svg.value) return

  // 如果你的 jianpu 是单参，就写 jianpu(parsed)；
  // 如果是双参，就写 jianpu(svg, parsed)。
  initApp(svg.value, {
    // jianpu, // ← 传入你自己的渲染函数
    url: new URL('../assets/tricolor.musicxml', import.meta.url).href // 如需覆盖资源 URL
  })

  // 如果需要的话，你也可以暴露清理逻辑：
  cleanup = () => {
    // 简单清理：把 svg 清空
    svg.value && svg.value.replaceChildren()
  }
})

onBeforeUnmount(() => {
  cleanup && cleanup()
})
</script>

<style scoped>
svg { display: block; }
</style>
