<template>
  <div class="text-center my-5">
    <h2>Status-Verteilung</h2>
    <canvas
        ref="chartCanvas"
        style="width: 100%; max-width: 500px; height: 300px; border: 1px solid #ccc;"
    ></canvas>
  </div>
</template>

<script setup>
import { ref, watch, onBeforeUnmount } from 'vue'
import { Chart } from 'chart.js/auto'

const props = defineProps({
  data: Object
})

const chartCanvas = ref(null)
let chartInstance = null

const createChart = () => {
  if (chartInstance) {
    chartInstance.destroy()
  }

  chartInstance = new Chart(chartCanvas.value, {
    type: 'bar',
    data: {
      labels: Object.keys(props.data),
      datasets: [
        {
          label: 'Anzahl',
          data: Object.values(props.data),
          backgroundColor: ['#28a745', '#dc3545', '#6c757d'],
          borderColor: '#00000010',
          borderWidth: 1
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      scales: {
        x: {
          title: {
            display: true,
            text: 'Status'
          },
          grid: {
            display: false
          }
        },
        y: {
          beginAtZero: true,
          title: {
            display: true,
            text: 'Anzahl'
          },
          ticks: {
            precision: 0
          },
          grid: {
            color: '#e0e0e0'
          }
        }
      },
      plugins: {
        legend: {
          display: false
        },
        tooltip: {
          callbacks: {
            label: context => `${context.parsed.y} Charaktere`
          }
        }
      }
    }
  })
}

watch(() => props.data, createChart, { deep: true, immediate: true })

onBeforeUnmount(() => {
  if (chartInstance) chartInstance.destroy()
})
</script>
