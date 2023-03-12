<script setup lang="ts">
  import {
    reactive,
    ref
  } from 'vue'
  import ElonTable from './components/ElonTable.vue'
  import ElonTableColumn from './components/ElonTableColumn.vue'

  const tableData = reactive([{
      date: '2016-05-03',
      name: 'Tom',
      address: 'No. 189, Grove St, Los Angeles',
    },
    {
      date: '2016-05-02',
      name: 'John',
      address: 'No. 189, Grove St, Los Angeles',
    },
    {
      date: '2016-05-04',
      name: 'Morgan',
      address: 'No. 189, Grove St, Los Angeles',
    },
    {
      date: '2016-05-01',
      name: 'Jessy',
      address: 'No. 189, Grove St, Los Angeles',
    },
  ])
  
  const search = ref('')
  
  const handleEdit = (index: number, row: object) => {
    console.log(index, row)
  }
  const handleDelete = (index: number, row: object) => {
    console.log(index, row)
  }
</script>

<template>
  <elon-table :data="tableData" style="width: 500px;" >
    <elon-table-column label="Date" prop="date" />
    <elon-table-column label="Name" prop="name" />
    <elon-table-column>
      <template #header>
        <el-input v-model="search" size="small" placeholder="Type to search" />
      </template>
      <template #default="scope">
        <el-button size="small" @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
        <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
      </template>
    </elon-table-column>
  </elon-table>
</template>
