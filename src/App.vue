<template>
  <div class="app-container">
    <header>
      <h1>Table Boilerplate - Car Models Database</h1>
      <p>An example of AG Grid Community Edition (v32) table with sorting and filtering</p>
      
      <!-- Dark mode toggle button -->
      <button 
        class="theme-toggle" 
        @click="toggleDarkMode" 
        :title="isDarkMode ? 'Switch to light mode' : 'Switch to dark mode'"
      >
        {{ isDarkMode ? '☀️' : '🌙' }}
      </button>
    </header>

    <div class="table-container" style="height: 500px;">
      <TableWrapper :dark-mode="isDarkMode" />
    </div>
    
    <footer>
      <p class="reference">Powered by <a href="https://www.ag-grid.com/" target="_blank" rel="noopener noreferrer">AG Grid</a></p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import TableWrapper from "./TableWrapper.vue";

// Dark mode state
const isDarkMode = ref(false);

// Toggle between light and dark mode
const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
};
</script>

<style scoped>
.app-container {
  margin: 0 1rem;
}

header {
  position: relative;
  padding-top: 1rem;
}

.table-container {
  margin-bottom: 1rem;
  width: 100%;
  /* Fixed height container as requested */
}

.theme-toggle {
  position: absolute;
  top: 1rem;
  right: 0;
  background: none;
  border: 1px solid #ccc;
  border-radius: 50%;
  width: 2.5rem;
  height: 2.5rem;
  font-size: 1.2rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.theme-toggle:hover {
  transform: scale(1.1);
}

footer {
  text-align: center;
  font-size: 0.9rem;
  padding: 1rem 0;
}

.reference a {
  color: #45a1ff;
  text-decoration: none;
}

.reference a:hover {
  text-decoration: underline;
}
</style>
