<template>
  <div>
    <h1>Gestión de Niveles de Carrera</h1>
    <form @submit.prevent="handleSubmit">
      <input v-model="datosForm.nombre" placeholder="Nombre del nivel" required />
      <button type="submit">{{ editMode ? 'Actualizar' : 'Agregar' }}</button>
    </form>
    <ul>
      <li v-for="nivel in niveles" :key="nivel.id">
        {{ nivel.nombre }}
        <button @click="editNivel(nivel)">Editar</button>
        <button @click="deleteNivel(nivel.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    const niveles = ref([]) // Array para almacenar niveles de carrera
    const datosForm = ref({ nombre: '' })
    const editMode = ref(false)
    const editId = ref(null)
    
    // La URL exacta que tu servidor JSON está mostrando
    const API_URL = 'http://localhost:3000/nivelesCarrera'

    const fetchNiveles = async () => {
      try {
        const response = await fetch(API_URL)
        if (!response.ok) throw new Error('Error al obtener datos')
        niveles.value = await response.json()
      } catch (error) {
        console.error('Error al cargar niveles:', error)
      }
    }

    const handleSubmit = async () => {
      try {
        if (editMode.value) {
          // Actualizar un registro existente (PUT)
          const response = await fetch(`${API_URL}/${editId.value}`, {
            method: 'PUT',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(datosForm.value)
          })
          
          if (!response.ok) throw new Error('Error al actualizar')
          
          // Recargar todos los niveles para asegurar sincronización
          await fetchNiveles()
          
          // Resetear el modo de edición
          editMode.value = false
          editId.value = null
        } else {
          // Agregar un nuevo registro (POST)
          const response = await fetch(API_URL, {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(datosForm.value)
          })
          
          if (!response.ok) throw new Error('Error al agregar')
          
          // Recargar todos los niveles para asegurar sincronización
          await fetchNiveles()
        }
        
        // Limpiar el formulario
        datosForm.value = { nombre: '' }
      } catch (error) {
        console.error('Error en el envío del formulario:', error)
        alert('Ocurrió un error. Intente nuevamente.')
      }
    }

    const editNivel = (nivel) => {
      datosForm.value = { nombre: nivel.nombre }
      editMode.value = true
      editId.value = nivel.id
    }

    const deleteNivel = async (id) => {
      try {
        const response = await fetch(`${API_URL}/${id}`, { 
          method: 'DELETE' 
        })
        
        if (!response.ok) throw new Error('Error al eliminar')
        
        // Recargar todos los niveles para asegurar sincronización
        await fetchNiveles()
      } catch (error) {
        console.error('Error al eliminar nivel:', error)
        alert('Error al eliminar. Intente nuevamente.')
      }
    }

    // Cargar los niveles al montar el componente
    onMounted(fetchNiveles)

    return {
      niveles,
      datosForm,
      editMode,
      handleSubmit,
      editNivel,
      deleteNivel,
    }
  },
}
</script>

<style scoped>
form {
  margin-bottom: 20px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}
li button {
  margin-left: 10px;
}
input {
  padding: 8px;
  margin-right: 10px;
}
button {
  padding: 8px 12px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
</style>