<template>
  <ag-grid-vue
    :columnDefs="colDefs"
    :rowData="rowData"
    class="ag-grid-container"
    :class="tableClass"
    @grid-ready="onGridReady"
    :defaultColDef="defaultColDef"
    :pagination="true"
    :paginationPageSize="20"
    :paginationPageSizeSelector="[10, 20, 50, 100]"
    >
  </ag-grid-vue>
</template>
  
<script setup>
// other
import { ref, reactive, onMounted } from 'vue';
import { faker } from '@faker-js/faker'; // Faker library

// ag grid
import { AgGridVue } from '@ag-grid-community/vue3'; // Vue component
import '@ag-grid-community/styles/ag-grid.css'; // mandatory
import '@ag-grid-community/styles/ag-theme-quartz.css'; // optional, but you need one (other options: ag-theme-alpine, ag-theme-balham, ag-theme-material, ag-theme-dark, ag-theme-fresh)
const tableClass = ref('ag-theme-quartz'); // this must match the theme you are using
import { ClientSideRowModelModule } from '@ag-grid-community/client-side-row-model';
import { ModuleRegistry } from '@ag-grid-community/core';
ModuleRegistry.registerModules([ClientSideRowModelModule]);

// Reactive state for AG Grid data and configuration
const rowData = ref(); // Holds the data for the grid rows
const colDefs = ref();  // Holds the column definitions
const gridApi = ref(null); // Reference to the AG Grid API (useful for interacting with the grid)

// Default Column Definitions: Applied to all columns unless overridden
const defaultColDef = {
  sortable: true,   // Enable sorting on all columns
  filter: true,     // Enable filtering on all columns
  resizable: true,  // Enable column resizing
  floatingFilter: true, // Show filter input row below headers
  flex: 1,          // Distribute column width evenly
  minWidth: 100,    // Minimum column width
};

// Function to generate fake car data
const generateFakeCarData = (count) => {
  const data = [];
  for (let i = 0; i < count; i++) {
    data.push({
      id: faker.string.uuid(),
      make: faker.vehicle.manufacturer(),
      model: faker.vehicle.model(),
      type: faker.vehicle.type(),
      year: faker.number.int({ min: 1990, max: new Date().getFullYear() + 1 }), // Year between 1990 and next year
      color: faker.vehicle.color(),
      vin: faker.vehicle.vin(),
      price: parseFloat(faker.finance.amount({ min: 5000, max: 80000, dec: 2 })), // Price with 2 decimal places
    });
  }
  return data;
};

// Define Columns
const setupColumns = () => {
  colDefs.value = [
    { field: 'make', headerName: 'Make', filter: 'agTextColumnFilter' }, // Specify text filter
    { field: 'model', headerName: 'Model' },
    { field: 'type', headerName: 'Type' },
    { field: 'year', headerName: 'Year', filter: 'agNumberColumnFilter' }, // Specify number filter
    { field: 'color', headerName: 'Color' },
    { field: 'vin', headerName: 'VIN' },
    {
      field: 'price',
      headerName: 'Price',
      filter: 'agNumberColumnFilter',
      valueFormatter: params => { // Format price as currency
        return '$' + params.value.toLocaleString(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 });
      }
    },
  ];
};

// Load Data
const setupData = () => {
  const rowCount = 2000; // Number of rows to generate
  rowData.value = generateFakeCarData(rowCount); // Generate and set the data
};

// Callback function when AG Grid is ready
const onGridReady = (params) => {
  gridApi.value = params.api; // Store the grid API
  console.log('AG Grid API ready:', gridApi.value);
  // Example: You could auto-size columns here if needed
  gridApi.value.autoSizeAllColumns();
};

// Load data when the component mounts
onMounted(async () => {
  setupColumns(); // Define the columns
  setupData(); // Load the data
});

</script>

<style scoped>
.ag-grid-container {
  height: 100%;
  width: 100%;
}
</style>
