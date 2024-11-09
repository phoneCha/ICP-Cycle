<script setup>
import { ref } from 'vue';
import { hello_world_backend } from 'declarations/hello_world_backend/index';

let greeting = ref('');
let names = ref([]);
let showNames = ref(false);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const name = target.querySelector('#name').value;

  await hello_world_backend.greet(name).then((response) => {
    greeting.value = response;
    target.querySelector('#name').value = ''; // Clear input field after submission
  });
}

async function toggleNames() {
  showNames.value = !showNames.value;

  if (showNames.value) {
    names.value = await hello_world_backend.getSubmittedNames();
  }
}
</script>

<template>
  <main class="container">
    <img src="/logo2.svg" alt="DFINITY logo" class="logo" />
    <form @submit="handleSubmit" class="form">
      <label for="name">Enter your name:</label>
      <input id="name" alt="Name" type="text" required />
      <button type="submit">Click Me!</button>
    </form>

    <section id="greeting" v-if="greeting">{{ greeting }}</section>

    <button @click="toggleNames" class="toggle-btn">
      {{ showNames ? 'Hide' : 'Show' }} All Names
    </button>

    <ul v-if="showNames" class="names-list">
      <li v-for="(name, index) in names" :key="index">{{ name }}</li>
    </ul>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
}

.logo {
  margin-bottom: 20px;
}

.form {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

input[type="text"] {
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  padding: 5px 10px;
  border-radius: 4px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

#greeting {
  margin-top: 15px;
  font-size: 18px;
  color: #333;
}

.toggle-btn {
  margin-top: 15px;
  padding: 5px 10px;
}

.names-list {
  list-style-type: none;
  padding: 0;
  margin-top: 10px;
}

.names-list li {
  padding: 5px;
  background-color: #f9f9f9;
  margin-bottom: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
</style>
