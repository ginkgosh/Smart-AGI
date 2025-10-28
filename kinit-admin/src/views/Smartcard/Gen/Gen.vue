<template>
  <div class="gen-container">
    <h1>{{ greeting }}</h1>
    <p class="subtitle">欢迎来到智能卡生成页面</p>

    <div class="card">
      <el-button type="primary" @click="showMessage"> 点击交互 </el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ElMessage } from 'element-plus'

const greeting = 'Hello World!'

const showMessage = () => {
  ElMessage.success('您已成功触发交互事件！')
}
</script>

<style scoped>
.gen-container {
  padding: 20px;
  text-align: center;
}

h1 {
  color: var(--el-color-primary);
  margin-bottom: 16px;
}

.subtitle {
  color: #666;
  margin-bottom: 24px;
}

.card {
  background: #fff;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: 0 auto;
}
</style>
