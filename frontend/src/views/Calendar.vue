<template>
  <div class="calendar-container">
    <el-card>
      <template #header>
        <div class="card-header">
          <span class="title">????</span>
        </div>
      </template>
      
      <el-calendar v-model="selectedDate">
        <template #date-cell="{ data }">
          <div class="calendar-cell">
            <div class="date">{{ data.day.split('-')[2] }}</div>
            <div v-if="getTaskCount(data.day)" class="task-badge">
              {{ getTaskCount(data.day) }} ???
            </div>
          </div>
        </template>
      </el-calendar>

      <div v-if="selectedDateTasks.length > 0" class="selected-date-tasks">
        <el-divider>{{ selectedDate.toLocaleDateString('zh-CN') }} ???</el-divider>
        <el-timeline>
          <el-timeline-item
            v-for="task in selectedDateTasks"
            :key="task.id"
            :timestamp="task.dueDate"
            :type="task.priority === 'high' ? 'danger' : task.priority === 'medium' ? 'warning' : 'primary'"
          >
            <el-card>
              <div class="task-item">
                <el-checkbox v-model="task.completed">
                  <span :class="{ 'completed-task': task.completed }">{{ task.title }}</span>
                </el-checkbox>
              </div>
              <p v-if="task.description" class="task-description">{{ task.description }}</p>
            </el-card>
          </el-timeline-item>
        </el-timeline>
      </div>
    </el-card>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedDate = ref(new Date())

// ??????
const tasks = ref([
  {
    id: 1,
    title: '????',
    description: '????????',
    priority: 'medium',
    dueDate: '2025-11-03 18:00',
    completed: false
  }
])

const getTaskCount = (date) => {
  return tasks.value.filter(task => {
    if (!task.dueDate) return false
    const taskDate = task.dueDate.split(' ')[0]
    return taskDate === date
  }).length
}

const selectedDateTasks = computed(() => {
  const dateStr = selectedDate.value.toISOString().split('T')[0]
  return tasks.value.filter(task => {
    if (!task.dueDate) return false
    const taskDate = task.dueDate.split(' ')[0]
    return taskDate === dateStr
  })
})
</script>

<style scoped>
.calendar-container {
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

.calendar-cell {
  text-align: center;
  padding: 4px;
}

.date {
  font-size: 16px;
  margin-bottom: 4px;
}

.task-badge {
  background-color: #409eff;
  color: white;
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 10px;
  display: inline-block;
}

.selected-date-tasks {
  margin-top: 20px;
}

.task-item {
  display: flex;
  align-items: center;
}

.task-description {
  color: #909399;
  font-size: 14px;
  margin: 8px 0 0 24px;
}

.completed-task {
  text-decoration: line-through;
  color: #909399;
}
</style>
