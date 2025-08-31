<!-- Composition API -->

<script setup lang="ts">
import { onMounted, ref } from 'vue';

const name = ref('Joh Doe 3');
const status = ref('pending');
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const newTask = ref('sdfsd');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending'
  }
  else if (status.value === 'pending') {
    status.value = 'inactive'
  }
  else {
    status.value = 'active'
  }
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};

const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();

    tasks.value = data.map((task: any) => task.title);

  } catch (error) {
    console.log('Error fetching tasks');
  }
});

</script>

<template>
  <h1>Vue jobs</h1>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add task</label>
    <input type="text" name="newTask" id="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>

  <!-- <button v-on:click="toggleStatus">Change status</button> -->
  <button @click="toggleStatus">Change status</button>

</template>

<style scoped></style>
