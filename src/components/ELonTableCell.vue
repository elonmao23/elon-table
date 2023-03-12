<template>
  <div class="elon-cell" :style="cellStyle" @mousedown.left="leftMouseDown">
    <slot>{{props.data[props.column.prop]}}</slot>
  </div>
</template>

<script setup lang="ts">
  import {
    inject,
    reactive
  } from 'vue'
  
  const props = defineProps({
    data: Object,
    rowIndex: Number,
    columnIndex: Number,
    column: Object
  })
  const tableProps = inject('tableProps')
  const selectedBorderStyle = inject('selectedBorderStyle')
  const cellStyle = reactive({
    'border-right': tableProps.border
  })
  
  function leftMouseDown(event) {
    const cellDom = event.target
    console.log(this)
    debugger
  }
</script>

<style scoped>
  .elon-cell {
    flex: 1;
  }
</style>