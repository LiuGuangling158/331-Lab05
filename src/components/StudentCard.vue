<script setup lang="ts">
import type { Student } from '@/types'

const props = defineProps<{
  student: Student
}>()
</script>

<template>
  <div class="border border-gray-300 rounded-lg p-4 m-2 bg-gray-50 min-w-[250px] shadow-sm">
    <div class="text-lg font-bold mb-2">{{ student.name }} {{ student.surname }}</div>
    <div class="text-base text-gray-600">GPA: {{ student.gpa.toFixed(2) }}</div>
  </div>
</template>

<style scoped>
</style>