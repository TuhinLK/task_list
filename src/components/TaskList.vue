<template>
  <div>
    <button @click="openNewTaskModal">Add Task</button>
    <table>
      <thead>
      <th>Task Name</th>
      <th>Time (in minutes)</th>
      <th>Actions</th>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td>{{ task.name }}</td>
        <td>{{ task.time }} minutes</td>
        <td>
          <button @click="deleteTask(index)">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
    <!-- New Task Modal -->
    <div v-if="isNewTaskModalOpen">
      <div class="modal">
        <h2>Add New Task</h2>
        <form @submit.prevent="addTask">
          <label for="newTaskName">Name:</label>
          <input v-model="newTask.name" type="text" id="newTaskName" required>
          <label for="newTaskTime">Time:</label>
          <input v-model="newTask.time" min="0" type="number" id="newTaskTime" required>
          <button type="submit">Submit</button>
        </form>
        <button @click="closeNewTaskModal">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  name: 'TaskList',
  setup() {
    const tasks = ref([{ name: 'Task 1', time: 60}, { name: 'Task 2', time: 75}]);
    const isModalOpen = ref(false);
    const isNewTaskModalOpen = ref(false);
    const newTask = ref({ name: '', time: 0 });
    const editedTask = ref({ name: '', time: 0 });
    let editedTaskIndex = null;

    const openEditModal = (index) => {
      editedTask.value = { ...tasks.value[index] };
      editedTaskIndex = index;
      isModalOpen.value = true;
    };
    const closeEditModal = () => {
      isModalOpen.value = false;
    };
    const updateTask = () => {
      tasks.value[editedTaskIndex] = { ...editedTask.value };
      closeEditModal();
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
    };

    const openNewTaskModal = () => {
      newTask.value = { name: '', time: 0 };
      isNewTaskModalOpen.value = true;
    };
    const closeNewTaskModal = () => {
      isNewTaskModalOpen.value = false;
    };
    const addTask = () => {
      tasks.value.push({ ...newTask.value });
      closeNewTaskModal();
    };
    return {
      tasks,
      isModalOpen,
      isNewTaskModalOpen,
      newTask,
      editedTask,
      openEditModal,
      closeEditModal,
      updateTask,
      deleteTask,
      openNewTaskModal,
      closeNewTaskModal,
      addTask,
    };
  },
};
</script>

<style>
/* Style your modal here */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background: white;
  z-index: 1000;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1em;
}

thead {
  background-color: #f8f9fa;
}

th {
  padding: 0.5em;
  border: 1px solid #0a0a0a;
  text-align: left;
  color: #0a0a0a;
}

td {
  padding: 0.5em;
  border: 1px solid #dee2e6;
  text-align: left;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.5em 1em;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
}

button:hover {
  background-color: #004085;
}
</style>