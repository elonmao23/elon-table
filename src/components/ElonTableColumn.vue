<template>
  <div class="elon-table-header" :style="columnHeaderStyle">
    <slot name="header">{{props.label}}</slot>
  </div>
</template>

<script setup lang="ts">
  import {
    reactive,
    inject,
    useSlots
  } from 'vue'

  const props = defineProps({
    label: String,
    prop: String
  })
  const tableProps = inject('tableProps')
  const columnList = inject('columnList')
  const slots = useSlots()

  columnList.push({
    ...props,
    defaultSlot: slots.default
  })

  const columnHeaderStyle = reactive({
    'border-right': tableProps.border
  })
</script>

<style scoped>
  .elon-table-header {
    flex: 1;
  }
</style>
