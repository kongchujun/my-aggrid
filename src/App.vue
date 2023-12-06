<template>
    <div id="app">
      <ag-grid-vue
        style="height: 400px; width: 600px;"
        class="ag-theme-alpine"
        :columnDefs="columnDefs"
        :rowData="rowData"
        @firstDataRendered="onFirstDataRendered"
        @cellValueChanged="onCellValueChanged"
      ></ag-grid-vue>
    </div>
  </template>
  
  <script>
  import { AgGridVue } from 'ag-grid-vue';
  import 'ag-grid-community/styles/ag-grid.css';
  import 'ag-grid-community/styles/ag-theme-alpine.css';
  
  export default {
    name: 'App',
    components: {
      AgGridVue,
    },
    data() {
      return {
        columnDefs: [
          { headerName: 'Name', field: 'name' },
          { headerName: 'Age', field: 'age' },
          { headerName: 'Salary', field: 'salary', editable: true },
          {
            headerName: 'Department',
            field: 'department',
            cellEditor: 'agSelectCellEditor',
            cellEditorParams: {
              values: ['IT', 'HR', 'Finance', 'Marketing'],
            },
          },
          // More columns...
        ],
        rowData: [
          { name: 'John Doe', age: 30, salary: 50000, department: 'IT' },
          { name: 'Jane Smith', age: 25, salary: 60000, department: 'HR' },
          // More rows...
        ],
      };
    },
    methods: {
      onFirstDataRendered(params) {
        params.api.sizeColumnsToFit();
      },
      onCellValueChanged(params) {
        if (params.column.colId === 'department') {
          const rowData = params.data;
          const selectedDepartment = params.newValue;
  
          // 根据选择的部门更新 "Salary" 列的可编辑状态
          const isSalaryEditable = selectedDepartment !== 'IT';
          params.api.setCellEditable(params.rowIndex, 'salary', isSalaryEditable);
        }
      },
    },
  };
  </script>
  
  <style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  
  .ag-theme-alpine {
    display: inline-block;
  }
  </style>
  