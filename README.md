# Table Boilerplate

A lightweight Vue 3 starter template that provides a fully functional, responsive data table using AG Grid Community Edition.

## Features

- ⚡️ [Vue 3](https://vuejs.org/) with Composition API
- 📊 [AG Grid Community Edition](https://www.ag-grid.com/) for powerful data tables
- 🔄 Built-in sorting and filtering capabilities
- 📑 Pagination support
- 🧩 Column resizing and flexible layouts
- 🎭 Fake data generation using [@faker-js/faker](https://fakerjs.dev/)
- 🚀 [Vite](https://vitejs.dev/) for fast development and builds
- 🎨 CSS styling with SCSS support

## Demo

This project demonstrates:
- A full-width, height-controlled data table displaying car information
- Text and number filtering options
- Column sorting functionality
- Pagination controls
- Currency formatting

## Getting Started

### Prerequisites

- Node.js 16+ and npm/yarn

### Installation

1. Clone the repository:

```bash
git clone https://github.com/your-username/table-boilerplate.git
cd table-boilerplate
```

2. Install dependencies:

```bash
npm install
# or
yarn
```

3. Start the development server:

```bash
npm run dev
# or
yarn dev
```

Your application will be available at `http://localhost:5173`

## Project Structure

```
table-boilerplate/
├─ src/
│  ├─ App.vue             # Main application wrapper
│  ├─ TableWrapper.vue    # AG Grid implementation component
│  ├─ styles.scss         # Global styles
│  ├─ main.js             # Application entry point
├─ index.html             # HTML template
├─ vite.config.js         # Vite configuration
└─ package.json           # Project dependencies and scripts
```

## Customizing the Table

### Adding or Modifying Columns

To customize the table columns, edit the `setupColumns` function in `TableWrapper.vue`:

```javascript
const setupColumns = () => {
  colDefs.value = [
    { field: 'make', headerName: 'Make', filter: 'agTextColumnFilter' },
    // Add or modify columns here
    { 
      field: 'newField', 
      headerName: 'New Column',
      filter: 'agTextColumnFilter' 
    },
  ];
};
```

### Changing Data Source

To use your own data instead of the generated fake data:

1. Replace the `setupData` function in `TableWrapper.vue`:

```javascript
const setupData = async () => {
  // Example with API call
  try {
    const response = await fetch('your-api-endpoint');
    const data = await response.json();
    rowData.value = data;
  } catch (error) {
    console.error('Error fetching data:', error);
    rowData.value = []; // Fallback to empty data
  }
};
```

### Adjusting Table Height

The table container has a fixed height that can be adjusted in `App.vue`:

```html
<div class="table-container" style="height: 500px;">
  <TableWrapper />
</div>
```

Change the `500px` value to your preferred height.

## Building for Production

```bash
npm run build
# or
yarn build
```

The build artifacts will be in the `dist` directory.

## Extending the Boilerplate

### Adding New Features

You can extend the table functionality with additional AG Grid features:

- Row selection
- Excel export
- Tree data
- Row grouping
- Cell editing

For these features, you may need to upgrade to AG Grid Enterprise or add community modules.

### Styling the Table

AG Grid provides multiple themes. To change the theme:

1. Import a different theme CSS in `TableWrapper.vue`:

```javascript
// Choose one of the following:
import '@ag-grid-community/styles/ag-theme-alpine.css';
// import '@ag-grid-community/styles/ag-theme-balham.css';
// import '@ag-grid-community/styles/ag-theme-material.css';
```

2. Update the `tableClass` ref in the same file:

```javascript
const tableClass = ref('ag-theme-alpine'); // Match the theme you imported
```

## License

[MIT](LICENSE)
