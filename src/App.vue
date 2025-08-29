<template>
  <div id="app">
    <h1>MusicXML 渲染示例</h1>
    <!-- 在这里使用你写的组件 -->
    <MusicXMLViewer />
  </div>
</template>

<script setup>
// 引入你写的组件
import MusicXMLViewer from './components/MusicXMLViewer.vue'
</script>

<style>
#app {
  width: 100vw;
  height: 100vh;
  overflow: auto;
}
</style>
