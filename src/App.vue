<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container">
        <a class="navbar-brand" href="#">
          <img src="./assets/vue.svg" alt="Vue Logo" width="30" height="30" class="d-inline-block align-text-top me-2">
          Mi Proyecto Vue
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link active" href="#">Niveles de Carrera</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    
    <main class="py-4">
      <NivelesCarrera />
    </main>

    <footer class="bg-dark text-white text-center py-3 mt-5">
      <div class="container">
        <p class="mb-0">Mi Proyecto Vue con Bootstrap &copy; {{ new Date().getFullYear() }}</p>
      </div>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import NivelesCarrera from './view/NivelesCarrera.vue'

export default defineComponent({
  name: 'App',
  components: {
    NivelesCarrera
  }
})
</script>