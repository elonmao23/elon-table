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
  
  function getCell(rowIndex: number, columnIndex: number) {
    return rows.value[rowIndex].cells[columnIndex]
  }
  
  const selectedInfo = reactive({
    selectedCell: null,
    lastHoverCell: null,
    edgeIndexes: computed(() => {
      const selectedCell = selectedInfo.selectedCell
      if (!selectedCell) {
        return null
      }
      const lastHoverCell = selectedInfo.lastHoverCell
      const rowIndex0 = selectedCell.props.rowIndex
      const columnIndex0 = selectedCell.props.columnIndex
      if (!lastHoverCell) {
        return {
          top: rowIndex0,
          bottom: rowIndex0,
          left: columnIndex0,
          right: columnIndex0
        }
      }
      const rowIndex1 = lastHoverCell.props.rowIndex
      const columnIndex1 = lastHoverCell.props.columnIndex
      const rowIndexes = rowIndex0 < rowIndex1 ? [rowIndex0, rowIndex1] : [rowIndex1, rowIndex0],
      const columnIndexes = columnIndex0 < columnIndex1 ? [columnIndex0, columnIndex1] : [columnIndex1, columnIndex0],
      return {
        top: rowIndexes[0],
        bottom: rowIndexes[1],
        left: columnIndexes[0],
        right: columnIndexes[1]
      }
    }),
    cornerCells: computed(() => {
      const edgeIndexes = selectedInfo.edgeIndexes
      if (!edgeIndexes) {
        return null
      }
      return {
        topLeft: getCell(edgeIndexes.top, edgeIndexes.left),
        bottomRight: getCell(edgeIndexes.bottom, edgeIndexes.right)
      }
    }),
    borderStyle: computed(() => {
      const cornerCells = selectedInfo.cornerCells
      if (!cornerCells) {
        return null
      }
      const topLeftCell = cornerCells.topLeft.$el
      const bottomRightCell = cornerCells.bottomRight.$el
      return {
        'box-shadow': props.selectedShadows,
        top: topLeftCell.offsetTop + 'px',
        left: topLeftCell.offsetLeft + 'px',
        height: bottomRightCell.offSetTop - topLeftCell.offSetTop + bottomRightCell.clientHeight + 'px',
        width: bottomRightCell.offSetLeft - topLeftCell.offSetLeft + bottomRightCell.clientWidth + 'px'
      }
    })
  })
  
  function setSelectedCell(rowIndex: number, columnIndex: number) {
    selectedInfo.selectedCell = getCell(rowIndex, columnIndex)
  }
  
  function setLastHoverCell(rowIndex: number, columnIndex: number) {
    selectedInfo.lastHoverCell = getCell(rowIndex, columnIndex)
  }
  
  function clearSelected() {
    selectedInfo.selectedCell = null
    selectedInfo.lastHoverCell = null
  }
  
  let dataImmer = null
  let tableData = shallowRef([])
  function initData(baseData) {
    dataImmer = new VueUndoRedo(BaseData.props.history)
    tableData.value = dataImmer.state.value
  }
  const emit = defineEmits(['change'])
  function onChange(patches) {
    if (patches) {
      clearSelected()
      tableData.value = dataImmer.state.value
      emit('change', patches)
    }
  }
  function updateData(updater: Function) {
    onChange(dataImmer.update(updater))
  }
  
  function undo() {
    onChange(dataImmer.undo(updater))
  }
  
  function redo() {
    onChange(dataImmer.redo(updater))
  }
  
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
