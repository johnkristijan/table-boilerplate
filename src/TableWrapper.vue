<template>
  <ag-grid-vue
    :columnDefs="colDefs"
    :rowData="rowData"
    class="ag-grid-container"
    :class="darkMode ? `${theme}-dark` : theme"
    @grid-ready="onGridReady"
    :defaultColDef="defaultColDef"
    :pagination="true"
    :paginationPageSize="20"
    :paginationPageSizeSelector="[10, 20, 50, 100]"
  >
  </ag-grid-vue>
</template>

<script setup>
// Import core dependencies
import { ref, onMounted, watch } from "vue";
import { faker } from "@faker-js/faker";

// AG Grid imports
import { AgGridVue } from "@ag-grid-community/vue3";
import { ClientSideRowModelModule } from "@ag-grid-community/client-side-row-model";
import { ModuleRegistry } from "@ag-grid-community/core";

// AG Grid styles
import "@ag-grid-community/styles/ag-grid.css";
import "@ag-grid-community/styles/ag-theme-quartz.css";
import "@ag-grid-community/styles/ag-theme-balham.css";
import "@ag-grid-community/styles/ag-theme-material.css";
import "@ag-grid-community/styles/ag-theme-alpine.css";

const theme = ref("ag-theme-quartz"); // select your theme here

// Register required AG Grid modules
ModuleRegistry.registerModules([ClientSideRowModelModule]);

// Props
const props = defineProps({
  darkMode: {
    type: Boolean,
    default: false,
  },
});

// Reactive state
const rowData = ref([]);
const colDefs = ref([]);
const gridApi = ref(null);

// Default column configuration
const defaultColDef = {
  sortable: true,
  filter: true,
  resizable: true,
  floatingFilter: true,
  flex: 1,
  minWidth: 100,
};

/**
 * Generates fake vehicle data for the table
 * @param {number} count - Number of records to generate
 * @returns {Array} Array of car data objects
 */
const generateFakeCarData = (count) => {
  const data = [];
  for (let i = 0; i < count; i++) {
    data.push({
      id: faker.string.uuid(),
      make: faker.vehicle.manufacturer(),
      model: faker.vehicle.model(),
      type: faker.vehicle.type(),
      year: faker.number.int({ min: 1990, max: new Date().getFullYear() + 1 }),
      color: faker.vehicle.color(),
      vin: faker.vehicle.vin(),
      price: parseFloat(
        faker.finance.amount({ min: 5000, max: 80000, dec: 2 })
      ),
      timestamp: faker.date.past(),
      date: faker.date.past().toISOString().split("T")[0],
    });
  }
  return data;
};

// Configure table columns with appropriate filters and formatters
const setupColumns = () => {
  colDefs.value = [
    { field: "make", headerName: "Make", filter: "agTextColumnFilter" },
    { field: "model", headerName: "Model", hide: false },
    { field: "type", headerName: "Type", hide: false },
    {
      field: "year",
      headerName: "Year",
      hide: true,
      filter: "agNumberColumnFilter",
    },
    { field: "color", headerName: "Color", hide: false },
    { field: "vin", headerName: "VIN", hide: true },
    {
      field: "timestamp",
      headerName: "Timestamp",
      filter: "agDateColumnFilter",
      valueFormatter: (params) => {
        // displayed value with date and time
        const date = new Date(params.value);
        return (
          date.toLocaleDateString("no-NO", {
            year: "numeric",
            month: "2-digit",
            day: "2-digit",
          }) +
          " " +
          date.toLocaleTimeString("no-NO", {
            hour: "2-digit",
            minute: "2-digit",
            hour12: false,
          })
        );
      },
    },
    {
      field: "date",
      headerName: "Date",
      filter: "agDateColumnFilter",
      valueFormatter: (params) => {
        // displayed value with date only
        const date = new Date(params.value);
        return date.toLocaleDateString("no-NO", {
          year: "numeric",
          month: "2-digit",
          day: "2-digit",
        });
      },
    },
    {
      field: "id",
      headerName: "ID",
      filter: "agTextColumnFilter",
      hide: true,
    },
    {
      field: "price",
      headerName: "Price",
      filter: "agNumberColumnFilter",
      valueFormatter: (params) => {
        return (
          "$" +
          params.value.toLocaleString(undefined, {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
          })
        );
      },
    },
  ];
};

// Load sample data into the table
const setupData = () => {
  rowData.value = generateFakeCarData(500); // Reduced from 2000 for better initial performance
  console.log("rowData.value :>> ", rowData.value);
};

// AG Grid initialization callback
const onGridReady = (params) => {
  gridApi.value = params.api;
  gridApi.value.sizeColumnsToFit();
};

// Initialize the table
onMounted(() => {
  setupColumns();
  setupData();
});

// Watch for dark mode changes and refresh the grid to properly apply theme
watch(
  () => props.darkMode,
  () => {
    if (gridApi.value) {
      setTimeout(() => {
        gridApi.value.refreshCells({ force: true });
      }, 0);
    }
  }
);
</script>

<style scoped>
.ag-grid-container {
  height: 100%;
  width: 100%;
}
</style>
