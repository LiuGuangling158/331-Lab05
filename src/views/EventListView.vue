<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import EventCategoryOrganizer from '@/components/EventCategoryOrganizer.vue'
import type { Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import { RouterLink, useRouter } from 'vue-router'
import EventService from '@/services/EventService'

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)
const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / 3)
  return page.value < totalPages
})

const props = defineProps({
  page: {
    type: Number,
    required: true
  }
})

const page = computed(() => props.page)
const router = useRouter()

onMounted((): void => {
  watchEffect(() => {
    EventService.getEvents(3, page.value)
          .then((response) => {
            events.value = response.data
            totalEvents.value = response.headers['x-total-count']
          })
          .catch(() => {
            router.push({ name: 'network-error-view' })
          })
  })
})
</script>

<template>
  <h1>Events For Good</h1>
  <!-- new element -->
  <div class="flex flex-col items-center">
    <div v-for="event in events" :key="event.id" class="mb-6 flex flex-col items-center">
      <EventCard :event="event" />
      <EventCategoryOrganizer :event="event" />
    </div>
    <div class="flex w-[290px]">
      <RouterLink
        id="page-prev"
        class="flex-1 text-left text-gray-700 no-underline"
        :to="{ name: 'event-list-view', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Prev Page</RouterLink
      >

      <RouterLink
        id="page-next"
        class="flex-1 text-right text-gray-700 no-underline"
        :to="{ name: 'event-list-view', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
        >Next Page &#62;</RouterLink
      >
    </div>
  </div>
</template>

<style scoped>
</style>
