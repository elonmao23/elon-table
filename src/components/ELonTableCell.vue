<template>
  <div class="elon-cell" :style="cellStyle" @mousedown.left="leftMouseDown" @mouseenter="mouseEnter">
    <slot>
      <input v-if="editing" :value="cellValue" @change="valueChange($event)" />
      <div v-else dbclick.left="leftDBClick">{{cellValue}}</div>
{{props.data[props.column.prop]}}
    </slot>
  </div>
</template>

<script setup lang="ts">
  import {
    defineExpose
    inject,
    reactive,
    computed,
    ref
  } from 'vue'
  
  const props = defineProps({
    data: Object,
    rowIndex: Number,
    columnIndex: Number,
    column: Object
  })
  const tableProps = inject('tableProps')
  const setSelectedCell = inject('setSelectedCell')
  const setLastHoverCell = inject('setLastHoverCell')
  const clearSelected = inject('clearSelected')
  const updateData = inject('updateData')
  const cellStyle = reactive({
    'border-right': tableProps.border
  })

  const cellValue = computed(() => props.data[props.column.prop])

  const editing = ref(false)
  
  function leftMouseDown(event) {
    clearSelected()
    setSelectedCell(props.rowIndex, props.columnIndex)
  }

  function mouseEnter(event: MouseEvent) {
    if (event.buttons !== 1) {
      return
    }
    setLastHoverCell(props.rowIndex, props.columnIndex)
  }

  function leftDBClick() {
    editing.value = true
  }

  function valueChange(event) {
    const target = event.target as HTMLInputElement
    updateData((draft: any) => draft[props.rowIndex][props.column.prop] = target.value)
  }

  defineExpose({
    props
  })
</script>

<style scoped>
  .elon-cell {
    flex: 1;
  }
</style>