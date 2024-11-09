<script setup lang="ts">
import { ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const titulo = ref('')
const sinopsis = ref('')
const director = ref('')
const categoria = ref('')
const temporadas = ref('')
const fechaEstreno = ref('')

async function crearSerie() {
  try {
    await http.post(ENDPOINT, {
      titulo: titulo.value,
      sinopsis: sinopsis.value,
      director: director.value,
      categoria: categoria.value,
      temporadas: parseInt(temporadas.value),
      fechaEstreno: fechaEstreno.value ? new Date(fechaEstreno.value) : null
    })
    router.push('/series')
  } catch (error) {
    console.error('Error al crear la serie:', error)
  }
}

function goBack() {
  router.go(-1)
}
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item"><RouterLink to="/series">Series</RouterLink></li>
        <li class="breadcrumb-item active" aria-current="page">Crear</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nueva Serie</h2>
    </div>

    <form @submit.prevent="crearSerie">
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
        <label for="categoria" class="col-sm-2 col-form-label">Categoría</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="categoria" v-model="categoria" required />
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

    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>
