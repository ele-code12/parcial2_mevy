<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import { useRouter, useRoute } from 'vue-router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const titulo = ref('')
const sinopsis = ref('')
const director = ref('')
const temporadas = ref('')
const fechaEstreno = ref('')
const router = useRouter()
const route = useRoute()
const id = route.params['id']

async function editarSerie() {
  try {
    const response = await http.patch(`${ENDPOINT}/${id}`, {
      titulo: titulo.value,
      sinopsis: sinopsis.value,
      director: director.value,
      temporadas: temporadas.value,
      fechaEstreno: fechaEstreno.value,
    
    })
    console.log('Update response:', response)
    router.push('/series')
  } catch (error) {
    console.error('Error updating series:', error)
    alert('Hubo un error al actualizar la serie. Por favor, inténtalo de nuevo.')
  }
}

async function getSerie() {
  try {
    const response = await http.get(`${ENDPOINT}/${id}`)
    titulo.value = response.data.titulo
    sinopsis.value = response.data.sinopsis
    director.value = response.data.director
    temporadas.value = response.data.temporadas
    fechaEstreno.value = response.data.fecha_estreno
    console.log('Fetched series data:', response.data)
  } catch (error) {
    console.error('Error fetching series:', error)
    alert('Hubo un error al obtener la información de la serie. Por favor, inténtalo de nuevo.')
  }
}

function goBack() {
  router.go(-1)
}

onMounted(() => {
  getSerie()
})
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/series">Series</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Editar</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Editar Serie</h2>
    </div>



    <div class="row">
     <form @submit.prevent="editarSerie">
      <div class="mb-3 row">
        <label for="titulo" class="col-sm-2 col-form-label">Título</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="titulo" v-model="titulo" required />
        </div>
      </div>
      <div class="mb-3 row">
        <label for="sinopsis" class="col-sm-2 col-form-label">Sinopsis</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="sinopsis" v-model="sinopsis" required />
        </div>
      </div>
      <div class="mb-3 row">
        <label for="director" class="col-sm-2 col-form-label">Director</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="director" v-model="director" required />
        </div>
      </div>
      <div class="mb-3 row">
        <label for="temporadas" class="col-sm-2 col-form-label">Temporadas</label>
        <div class="col-sm-10">
          <input type="number" class="form-control" id="temporadas" v-model="temporadas" required />
        </div>
      </div>
      <div class="mb-3 row">
        <label for="fechaEstreno" class="col-sm-2 col-form-label">Fecha de Estreno</label>
        <div class="col-sm-10">
          <input type="date" class="form-control" id="fechaEstreno" v-model="fechaEstreno" required />
        </div>
      </div>
      <div class="text-center mt-3">
        <button type="submit" class="btn btn-primary btn-lg">Crear</button>
      </div>
    </form>
</div>
    </div>
</template>

<style scoped></style>
