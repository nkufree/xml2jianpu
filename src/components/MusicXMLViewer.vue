<template>
  <div class="page-wrap">
    <div class="toolbar">
      <label class="btn">
        <input type="file" accept=".musicxml,.xml,application/xml" @change="onFileChange" hidden />
        上传 MusicXML
      </label>
      <button class="btn" @click="reloadDefault">加载内置示例</button>
    </div>

    <!-- 不再限制高度、不加 overflow；让它随内容自然增长 -->
    <div class="canvas-wrap">
      <svg ref="svg" class="score-svg"></svg>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import initApp from './MusicXMLViewer.js' // 你的 init 函数：支持 url / xmlString / jianpu

const svg = ref(null)

async function fitSvgHeight(svgEl, padding = 16) {
  // 等 D3 渲染完成后再测量
  await nextTick()
  // 计算内容包围盒（仅针对绘制在 <svg> 里的元素）
  const bbox = svgEl.getBBox()
  const contentHeight = Math.max(0, Math.ceil(bbox.y + bbox.height + padding))
  // 宽度可保持 100%，高度用像素撑开文档流，让页面产生滚动
  svgEl.removeAttribute('viewBox') // 可选：若你只用高度控制布局，不需要 viewBox
  svgEl.setAttribute('height', contentHeight || 1) // 避免 0 高
}

async function renderWithUrl(url) {
  if (!svg.value) return
  await initApp(svg.value, url)
  await fitSvgHeight(svg.value)
}

async function renderWithXmlString(xmlString) {
  if (!svg.value) return
  await initApp(svg.value, xmlString)
  await fitSvgHeight(svg.value)
}

function reloadDefault() {
  const url = new URL('../assets/tricolor.musicxml', import.meta.url).href
  renderWithUrl(url)
}

async function onFileChange(e) {
  const file = e.target.files?.[0]
  if (!file) return
  try {
    const text = await file.text()
    console.log('text', text)
    await renderWithXmlString(text)
  } finally {
    e.target.value = ''
  }
}

onMounted(() => {
  reloadDefault()
})
</script>

<style scoped>
.page-wrap {
  display: flex;
  flex-direction: column;
  gap: 12px;
  /* 不强制高度，交给内容决定；让页面整体滚动 */
}

.toolbar {
  display: flex;
  gap: 8px;
  align-items: center;
}

.btn {
  padding: 8px 12px;
  border: 1px solid #dcdcdc;
  border-radius: 8px;
  background: #fff;
  cursor: pointer;
  transition: .15s;
  font-size: 14px;
}
.btn:hover { background: #f7f7f7; }

/* 不是滚动容器，只是普通块级包裹 */
.canvas-wrap {
  width: 100%;
}

/* 宽度响应布局，高度由 fitSvgHeight 用像素设置 */
.score-svg {
  display: block;
  width: 100%;
  height: auto; /* 初始 auto；渲染后我们会写死具体像素高度 */
}
</style>
