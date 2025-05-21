<template>
  <div class="container py-4">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header bg-primary text-white">
            <h2 class="mb-0">Gestión de Niveles de Carrera</h2>
          </div>
          <div class="card-body">
            <form @submit.prevent="handleSubmit" class="mb-4">
              <div class="input-group">
                <input 
                  v-model="datosForm.nombre" 
                  class="form-control" 
                  placeholder="Nombre del nivel" 
                  required 
                />
                <button 
                  type="submit" 
                  class="btn btn-primary"
                >
                  {{ editMode ? 'Actualizar' : 'Agregar' }}
                </button>
              </div>
            </form>
            
            <div class="table-responsive">
              <table class="table table-striped table-hover">
                <thead class="table-dark">
                  <tr>
                    <th>Nombre</th>
                    <th>Acciones</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="nivel in niveles" :key="nivel.id">
                    <td>{{ nivel.nombre }}</td>
                    <td>
                      <button 
                        @click="editNivel(nivel)" 
                        class="btn btn-sm btn-warning me-2"
                      >
                        <i class="bi bi-pencil"></i> Editar
                      </button>
                      <button 
                        @click="deleteNivel(nivel.id)" 
                        class="btn btn-sm btn-danger"
                      >
                        <i class="bi bi-trash"></i> Eliminar
                      </button>
                    </td>
                  </tr>
                  <tr v-if="niveles.length === 0">
                    <td colspan="2" class="text-center">No hay niveles disponibles</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
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
/* Los estilos personalizados adicionales pueden ir aquí */
</style>