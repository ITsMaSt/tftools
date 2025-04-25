<template>
  <div class="row">
    <div class="col-md-4 mb-4" v-for="character in characters" :key="character.id">
      <div class="card h-100">
        <img :src="character.image" class="card-img-top" :alt="character.name" />
        <div class="card-body">
          <h5 class="card-title">{{ character.name }}</h5>
          <p class="card-text">Status: {{ character.status }}</p>
          <button class="btn btn-primary" @click="openModal(character)">Mehr Infos</button>
        </div>
      </div>
    </div>

    <!-- Modal für Charakterdetails -->
    <div class="modal fade" id="characterModal" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content" v-if="selectedCharacter">
          <div class="modal-header">
            <h5 class="modal-title">{{ selectedCharacter.name }}</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Schließen"></button>
          </div>
          <div class="modal-body">
            <img :src="selectedCharacter.image" class="img-fluid mb-3" alt="Bild des Charakters" />
            <p><strong>Spezies:</strong> {{ selectedCharacter.species }}</p>
            <p><strong>Herkunft:</strong> {{ selectedCharacter.origin?.name || 'Unbekannt' }}</p>
            <p><strong>Aktueller Ort:</strong> {{ selectedCharacter.location?.name || 'Unbekannt' }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, defineEmits } from 'vue'
import 'bootstrap'

const characters = ref([])
const selectedCharacter = ref(null)

const emit = defineEmits(['status-counts'])

const fetchCharacters = async () => {
  try {
    const response = await fetch('https://rickandmortyapi.com/api/character')
    const data = await response.json()
    characters.value = data.results

    const statusCounter = { alive: 0, dead: 0, unknown: 0 }

    data.results.forEach(character => {
      const status = (character.status || '').toLowerCase()

      if (status === 'alive') statusCounter.alive++
      else if (status === 'dead') statusCounter.dead++
      else statusCounter.unknown++
    })

    emit('status-counts', {
      Alive: statusCounter.alive,
      Dead: statusCounter.dead,
      unknown: statusCounter.unknown
    })
  } catch (error) {
    console.error('Fehler beim Laden der Charaktere:', error)
  }
}

const openModal = (character) => {
  selectedCharacter.value = character
  const modal = new bootstrap.Modal(document.getElementById('characterModal'))
  modal.show()
}

onMounted(fetchCharacters)
</script>
