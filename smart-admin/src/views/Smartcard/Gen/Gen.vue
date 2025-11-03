<template>
  <div class="gen-container">
    <!-- 使用Flex布局实现左右分区 -->
    <div class="layout-wrapper">
      <!-- 左侧区域 -->
      <div class="left-panel">
        <div class="panel-header">
          <h2 class="left-aligned">卡片设计区</h2>
        </div>
        <div class="card">
          <el-form :model="form" label-width="80px">
            <el-form-item label="姓名">
              <el-input v-model="form.name" placeholder="请输入姓名" />
            </el-form-item>
            <el-form-item label="邮箱">
              <el-input v-model="form.email" placeholder="请输入邮箱" />
            </el-form-item>
            <el-form-item label="描述">
              <el-input
                v-model="form.description"
                type="textarea"
                placeholder="请输入描述"
                :rows="4"
              />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm">提交</el-button>
              <el-button @click="resetForm">重置</el-button>
            </el-form-item>
          </el-form>
        </div>
      </div>

      <!-- 右侧区域 -->
      <div class="right-panel">
        <div class="panel-header">
          <h2 class="left-aligned">卡片预览区</h2>
        </div>
        <div class="card">
          <div class="info-display">
            <h3>用户信息预览</h3>
            <el-descriptions :column="1" border>
              <el-descriptions-item label="姓名">{{ form.name || '暂无' }}</el-descriptions-item>
              <el-descriptions-item label="邮箱">{{ form.email || '暂无' }}</el-descriptions-item>
              <el-descriptions-item label="描述">{{
                form.description || '暂无'
              }}</el-descriptions-item>
            </el-descriptions>
          </div>

          <div class="action-buttons">
            <el-button type="success" @click="showMessage">点击交互</el-button>
            <el-button type="warning" @click="clearInfo">清空信息</el-button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { ElMessage } from 'element-plus'

// 表单数据
const form = reactive({
  name: '',
  email: '',
  description: ''
})

// 提交表单
const submitForm = () => {
  if (!form.name || !form.email) {
    ElMessage.warning('请填写必填项！')
    return
  }
  ElMessage.success('表单提交成功！')
}

// 重置表单
const resetForm = () => {
  form.name = ''
  form.email = ''
  form.description = ''
}

// 清空信息
const clearInfo = () => {
  resetForm()
  ElMessage.info('信息已清空')
}

// 显示消息
const showMessage = () => {
  ElMessage.success('您已成功触发交互事件！')
}
</script>

<style scoped>
.gen-container {
  padding: 20px;
  width: 100%;
  height: 100%;
}

.layout-wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  width: 100%;
  min-height: calc(100vh - 150px);
}

.left-panel,
.right-panel {
  flex: 1;
  min-width: 300px;
  display: flex;
  flex-direction: column;
}

.panel-header h2.left-aligned {
  color: var(--el-color-primary);
  margin-bottom: 16px;
  text-align: left;
}

.card {
  background: #fff;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  flex: 1;
  display: flex;
  flex-direction: column;
}

.card .el-form {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.info-display {
  margin-bottom: 20px;
  flex: 1;
}

.action-buttons {
  display: flex;
  justify-content: center;
  gap: 10px;
}

/* 响应式设计 - 当屏幕宽度小于 768px 时，改为上下布局 */
@media (max-width: 768px) {
  .layout-wrapper {
    flex-direction: column;
  }

  .left-panel,
  .right-panel {
    width: 100%;
  }
}

/* 确保在大屏幕上始终显示为左右布局 */
@media (min-width: 769px) {
  .left-panel,
  .right-panel {
    width: calc(50% - 10px);
  }
}
</style>
