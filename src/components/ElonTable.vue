<template>
  <div style="display: flex; flex-direction: column;" :style="tableStyle">
    <div style="display: flex;" :style="tableHeaderStyle">
      <slot></slot>
    </div>
    <div class="elon-tbody">
      <elon-table-row v-for="(rowData, index) in props.data" :data="rowData" :index="index">
      </elon-table-row>
      <div class="selected-border" :style="selectedBorderStyle"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import {
    reactive,
    provide,
    computed
  } from 'vue'
  import ElonTableRow from './ElonTableRow.vue'

  const props = defineProps({
    data: Object,
    border: {
      type: String,
      default: '1px solid #DCDFE6'
    },
    selectedShadow: {
      type: String,
      default: '0 0 1px 1px #409EFF'
    }
  })

  const columnList = reactive([])
  
  const selectedInfo = {
    selectedCell: null,
    lastHoverCell: null,
    cellOnLeft: computed(() => {
      if () {
        
      }
    }Math.min(selectedInfo.selectedCell.props.columnIndex,
    selectedInfo.lastHoverCell.props.columnIndex) )
  }
  
  const selectedBorderStyle = reactive({
    'box-shadow': props.selectedShadow,
    // top: computed(() => Math.min(selectedCell.offsetTop, selectedCellEnd.offsetTop)),
    // left: computed(() => Math.min(selectedCell.offsetLeft, selectedCellEnd.offsetLeft)),
    // width: computed(() => Math.min(selectedCell.offsetLeft, selectedCellEnd.offsetLeft)),
    // height: computed(() => Math.min(selectedCell.offsetLeft, selectedCellEnd.offsetLeft))
  })
  
  const tableHeaderStyle = reactive({
    'border-bottom': props.border
  })
  const tableStyle = reactive({
    'border-top': props.border,
    'border-left': props.border
  })
  provide('columnList', columnList)
  provide('tableProps', props)
  provide('selectedBorderStyle', selectedBorderStyle)
  
</script>

<style scoped>
  .elon-tbody {
    position: relative;
    display: flex;
    flex-direction: column;
  }
  
  .selected-border {
    position: absolute;
  }
</style>
