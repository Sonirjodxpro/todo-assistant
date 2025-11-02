<template>
  <div class="tasks-container">
    <el-card class="task-card">
      <template #header>
        <div class="card-header">
          <span class="title">????</span>
          <el-button type="primary" :icon="Plus" @click="showAddDialog = true">
            ????
          </el-button>
        </div>
      </template>
      
      <el-empty v-if="tasks.length === 0" description="??????">
        <el-button type="primary" @click="showAddDialog = true">???????</el-button>
      </el-empty>
      
      <el-timeline v-else>
        <el-timeline-item
          v-for="task in tasks"
          :key="task.id"
          :timestamp="task.dueDate"
          placement="top"
          :type="task.priority === 'high' ? 'danger' : task.priority === 'medium' ? 'warning' : 'primary'"
        >
          <el-card>
            <div class="task-item">
              <el-checkbox v-model="task.completed" @change="toggleTask(task)">
                <span :class="{ 'completed-task': task.completed }">{{ task.title }}</span>
              </el-checkbox>
              <div class="task-actions">
                <el-button type="primary" :icon="Edit" circle size="small" @click="editTask(task)" />
                <el-button type="danger" :icon="Delete" circle size="small" @click="deleteTask(task.id)" />
              </div>
            </div>
            <p v-if="task.description" class="task-description">{{ task.description }}</p>
          </el-card>
        </el-timeline-item>
      </el-timeline>
    </el-card>

    <!-- ??/??????? -->
    <el-dialog
      v-model="showAddDialog"
      :title="editingTask ? '????' : '????'"
      width="90%"
    >
      <el-form :model="taskForm" label-width="80px">
        <el-form-item label="????">
          <el-input v-model="taskForm.title" placeholder="???????" />
        </el-form-item>
        <el-form-item label="????">
          <el-input
            v-model="taskForm.description"
            type="textarea"
            :rows="3"
            placeholder="???????"
          />
        </el-form-item>
        <el-form-item label="???">
          <el-select v-model="taskForm.priority" placeholder="??????">
            <el-option label="?" value="high" />
            <el-option label="?" value="medium" />
            <el-option label="?" value="low" />
          </el-select>
        </el-form-item>
        <el-form-item label="????">
          <el-date-picker
            v-model="taskForm.dueDate"
            type="datetime"
            placeholder="??????"
            format="YYYY-MM-DD HH:mm"
            value-format="YYYY-MM-DD HH:mm"
          />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="showAddDialog = false">??</el-button>
        <el-button type="primary" @click="saveTask">??</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Plus, Edit, Delete } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'

const tasks = ref([
  {
    id: 1,
    title: '????',
    description: '??????????????????',
    priority: 'medium',
    dueDate: '2025-11-03 18:00',
    completed: false
  }
])

const showAddDialog = ref(false)
const editingTask = ref(null)
const taskForm = ref({
  title: '',
  description: '',
  priority: 'medium',
  dueDate: ''
})

const toggleTask = (task) => {
  ElMessage.success(task.completed ? '?????' : '?????????')
}

const editTask = (task) => {
  editingTask.value = task
  taskForm.value = { ...task }
  showAddDialog.value = true
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
  ElMessage.success('?????')
}

const saveTask = () => {
  if (!taskForm.value.title) {
    ElMessage.warning('???????')
    return
  }

  if (editingTask.value) {
    Object.assign(editingTask.value, taskForm.value)
    ElMessage.success('?????')
  } else {
    tasks.value.push({
      id: Date.now(),
      ...taskForm.value,
      completed: false
    })
    ElMessage.success('?????')
  }

  showAddDialog.value = false
  editingTask.value = null
  taskForm.value = {
    title: '',
    description: '',
    priority: 'medium',
    dueDate: ''
  }
}
</script>

<style scoped>
.tasks-container {
  padding: 16px;
  min-height: calc(100vh - 120px);
}

.task-card {
  margin-bottom: 16px;
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

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
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

.task-actions {
  display: flex;
  gap: 8px;
}
</style>
