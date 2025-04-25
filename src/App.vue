<template>
  <div class="container py-5">
    <h1 class="mb-4">Rick and Morty Charaktere</h1>
    <CharacterList @status-counts="updateCounts" />
    <StatusChart
        v-if="Object.keys(statusCounts).length > 0"
        :data="statusCounts"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import CharacterList from './components/CharacterList.vue'
import StatusChart from './components/StatusChart.vue'

const statusCounts = ref({ Alive: 0, Dead: 0, unknown: 0 })

const updateCounts = (counts) => {
  statusCounts.value = counts
}
</script>
