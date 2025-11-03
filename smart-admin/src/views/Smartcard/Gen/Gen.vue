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
          <!-- 第一部分：输入您的需求 -->
          <div class="section">
            <h3>输入您的需求</h3>
            <el-input
              v-model="requirement"
              type="textarea"
              placeholder="请输入您的需求"
              :rows="4"
              style="width: 100%"
            />
          </div>

          <!-- 第二部分：选择模型 -->
          <div class="section">
            <h3>选择模型</h3>
            <el-select v-model="selectedModel" placeholder="请选择模型" style="width: 100%">
              <el-option
                v-for="model in modelOptions"
                :key="model.value"
                :label="model.label"
                :value="model.value"
              />
            </el-select>
          </div>

          <!-- 第三部分：生成按钮 -->
          <div class="section">
            <el-button
              type="primary"
              @click="generateContent"
              :loading="loading"
              style="width: 100%"
            >
              根据需求生成
            </el-button>
          </div>

          <!-- 第四部分：展示后端返回内容 -->
          <div v-if="resultContent" class="section">
            <h3>生成结果</h3>
            <div class="result-content">
              {{ resultContent }}
            </div>
          </div>
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
import axios from 'axios'

// 新增需求相关的数据
const requirement = ref('')
const selectedModel = ref('')
const resultContent = ref('')
const loading = ref(false)

// 模型选项
const modelOptions = [
  { label: '模型A', value: 'model_a' },
  { label: '模型B', value: 'model_b' },
  { label: '模型C', value: 'model_c' }
]

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

// 根据需求生成内容
const generateContent = async () => {
  if (!requirement.value) {
    ElMessage.warning('请输入您的需求！')
    return
  }

  if (!selectedModel.value) {
    ElMessage.warning('请选择模型！')
    return
  }

  loading.value = true
  try {
    // 调用后端API
    const response = await axios.post('http://127.0.0.1:8080/api/gen', {
      requirement: requirement.value,
      model: selectedModel.value
    })

    // 展示返回的内容
    resultContent.value =
      response.data.content || response.data.result || JSON.stringify(response.data)
    ElMessage.success('生成成功！')
  } catch (error) {
    console.error('生成失败:', error)
    ElMessage.error('生成失败，请稍后重试！')
    resultContent.value = '生成失败，请稍后重试！'
  } finally {
    loading.value = false
  }
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

.section {
  margin-bottom: 20px;
}

.section h3 {
  margin-bottom: 10px;
  color: #333;
}

.result-content {
  background-color: #f5f5f5;
  border-radius: 4px;
  padding: 12px;
  min-height: 100px;
  white-space: pre-wrap;
  word-wrap: break-word;
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
