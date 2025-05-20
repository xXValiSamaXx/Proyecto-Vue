<template>
  <NivelesCarrera />
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