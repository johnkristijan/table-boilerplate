<template>
  <div class="app-container">
    <h1>Table Boilerplate - Car Models Database</h1>
    <p>An example of AG Grid Community Edition table with sorting and filtering</p>

    <div class="table-container">
      <TableWrapper />
    </div>
    
    <footer>
      <p class="reference">Powered by <a href="https://www.ag-grid.com/" target="_blank" rel="noopener noreferrer">AG Grid</a></p>
    </footer>
  </div>
</template>

<script setup>
import TableWrapper from "./TableWrapper.vue";
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  height: 100dvh;
  margin-left: 1rem;
  margin-right: 1rem;
}

.table-container {
  flex: 1;
  min-height: 0; /* Important for flex child to properly shrink */
  margin-bottom: 20px;
  max-width: 100%;
  max-height: 90%;
}

footer {
  margin-top: auto;
  text-align: center;
  font-size: 0.9rem;
  padding: 10px 0;
}

.reference a {
  color: #45a1ff;
  text-decoration: none;
}

.reference a:hover {
  text-decoration: underline;
}
</style>
