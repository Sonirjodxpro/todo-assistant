<template>
  <div class="settings-container">
    <el-card>
      <template #header>
        <div class="card-header">
          <span class="title">????</span>
        </div>
      </template>
      
      <el-form :model="settings" label-width="120px">
        <el-divider content-position="left">????</el-divider>
        
        <el-form-item label="?????">
          <el-switch v-model="settings.browserNotification" />
        </el-form-item>
        
        <el-form-item label="??????">
          <el-select v-model="settings.reminderTime" placeholder="???????">
            <el-option label="5??" :value="5" />
            <el-option label="10??" :value="10" />
            <el-option label="15??" :value="15" />
            <el-option label="30??" :value="30" />
            <el-option label="1??" :value="60" />
          </el-select>
        </el-form-item>

        <el-divider content-position="left">????</el-divider>
        
        <el-form-item label="??????">
          <el-switch v-model="settings.feishuEnabled" />
        </el-form-item>
        
        <el-form-item label="Webhook??">
          <el-input
            v-model="settings.feishuWebhook"
            placeholder="?????Webhook??"
            :disabled="!settings.feishuEnabled"
          />
        </el-form-item>

        <el-form-item label="???">
          <el-input
            v-model="settings.feishuGroup"
            placeholder="????????"
            :disabled="!settings.feishuEnabled"
          />
        </el-form-item>

        <el-divider content-position="left">????</el-divider>
        
        <el-form-item label="???">
          <el-input v-model="settings.username" placeholder="??????" />
        </el-form-item>
        
        <el-form-item label="??">
          <el-input v-model="settings.email" placeholder="?????" />
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="saveSettings">????</el-button>
          <el-button @click="resetSettings">??</el-button>
        </el-form-item>
      </el-form>
    </el-card>

    <el-card style="margin-top: 16px;">
      <template #header>
        <div class="card-header">
          <span class="title">??</span>
        </div>
      </template>
      
      <el-descriptions :column="1" border>
        <el-descriptions-item label="????">???? H5 ??</el-descriptions-item>
        <el-descriptions-item label="??">v1.0.0</el-descriptions-item>
        <el-descriptions-item label="???">Vue 3 + Vite + Element Plus</el-descriptions-item>
        <el-descriptions-item label="??">
          ???? H5 Web ???????????????????????????
        </el-descriptions-item>
      </el-descriptions>
    </el-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'

const settings = ref({
  browserNotification: true,
  reminderTime: 10,
  feishuEnabled: false,
  feishuWebhook: '',
  feishuGroup: '',
  username: '',
  email: ''
})

const saveSettings = () => {
  // TODO: ????????????
  localStorage.setItem('todoAppSettings', JSON.stringify(settings.value))
  ElMessage.success('?????')
}

const resetSettings = () => {
  settings.value = {
    browserNotification: true,
    reminderTime: 10,
    feishuEnabled: false,
    feishuWebhook: '',
    feishuGroup: '',
    username: '',
    email: ''
  }
  ElMessage.info('?????')
}

// ???????
const loadSettings = () => {
  const saved = localStorage.getItem('todoAppSettings')
  if (saved) {
    settings.value = JSON.parse(saved)
  }
}

loadSettings()
</script>

<style scoped>
.settings-container {
  padding: 16px;
  min-height: calc(100vh - 120px);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.title {
  font-size: 18px;
  font-weight: bold;
}
</style>
