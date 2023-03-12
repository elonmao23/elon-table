<template>
  <div class="table-row" :style="rowStyle">
    <elon-table-cell v-for="(column, columnIndex) in columnList"
      :data="props.data"
      :rowIndex="props.index"
      :columnIndex="columnIndex"
      :column="column">
      <component :is="column.defaultSlot" :$index="props.index" :row="props.data"></component>
    </elon-table-cell>
  </div>
</template>

<script setup lang="ts">
  import {
    reactive,
    inject
  } from 'vue'
  import ElonTableCell from './ELonTableCell.vue'

  const props = defineProps({
    data: Object,
    index: Number
  })

  const columnList = inject('columnList')
  const tableProps = inject('tableProps')
  
  const rowStyle = reactive({
    'border-bottom': tableProps.border
  })
</script>

<style scoped>
  .table-row {
    display: flex;
  }
</style>
