<script setup lang="ts">
import { ref, onMounted } from 'vue'
import StudentCard from '@/components/StudentCard.vue'
import type { Student } from '@/types'
import StudentService from '@/services/StudentService'

const students = ref<Student[]>([])

onMounted(() => {
  StudentService.getStudents()
    .then((response) => {
      console.log('Student data fetched:', response.data)
      students.value = response.data
    })
    .catch((error) => {
      console.error('Error fetching student data:', error)
    })
})
</script>

<template>
  <div class="max-w-[1200px] mx-auto p-5">
    <h1>Student Information</h1>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5 justify-items-center" v-if="students.length > 0">
      <StudentCard 
        v-for="(student, index) in students" 
        :key="index" 
        :student="student" 
      />
    </div>
    <div v-else class="text-center text-lg text-gray-500 mt-12">Loading student data...</div>
  </div>
</template>

<style scoped>
</style>