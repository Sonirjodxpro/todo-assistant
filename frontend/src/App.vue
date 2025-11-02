<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const activeMenu = ref('tasks')

const handleMenuClick = (menu) => {
  activeMenu.value = menu
  router.push(`/${menu}`)
}
</script>

<template>
  <div class="app-container">
    <!-- ????? -->
    <div class="main-content">
      <router-view />
    </div>

    <!-- ????? -->
    <div class="bottom-nav">
      <div
        class="nav-item"
        :class="{ active: activeMenu === 'tasks' }"
        @click="handleMenuClick('tasks')"
      >
        <el-icon :size="24">
          <List />
        </el-icon>
        <span>??</span>
      </div>
      <div
        class="nav-item"
        :class="{ active: activeMenu === 'calendar' }"
        @click="handleMenuClick('calendar')"
      >
        <el-icon :size="24">
          <Calendar />
        </el-icon>
        <span>??</span>
      </div>
      <div
        class="nav-item"
        :class="{ active: activeMenu === 'settings' }"
        @click="handleMenuClick('settings')"
      >
        <el-icon :size="24">
          <Setting />
        </el-icon>
        <span>??</span>
      </div>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.app-container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
}

.main-content {
  flex: 1;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  padding-bottom: 60px;
}

.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60px;
  background-color: #ffffff;
  border-top: 1px solid #e4e7ed;
  display: flex;
  justify-content: space-around;
  align-items: center;
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s;
  color: #909399;
  padding: 8px 0;
}

.nav-item span {
  font-size: 12px;
  margin-top: 4px;
}

.nav-item.active {
  color: #409eff;
}

.nav-item:active {
  background-color: #f5f7fa;
}
</style>
