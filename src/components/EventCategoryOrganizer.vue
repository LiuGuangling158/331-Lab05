<script setup lang="ts">
import type { Event } from '@/types'

defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="w-[250px] mb-[18px]">
    <div class="text-right">
      <span class="block text-sm mb-1">{{ event.category }}</span>
      <span class="block text-sm">{{ event.organizer }}</span>
    </div>
  </div>
</template>

<style scoped>
</style>