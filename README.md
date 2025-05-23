# Mi Proyecto Vue

Aplicación web desarrollada con Vue 3, TypeScript, Vite y Bootstrap para la gestión de niveles de carrera académica.

## Requisitos previos

- Node.js (versión 18 o superior)
- npm (incluido con Node.js)
- json-server (instalado globalmente o mediante scripts del proyecto)

## Tecnologías utilizadas

- Vue 3 con Composition API
- TypeScript
- Vite (empaquetador y servidor de desarrollo)
- Bootstrap 5 (framework CSS)
- JSON Server (API REST simulada)

## Configuración del proyecto

1. **Clonar el repositorio**

```bash
git clone <url-del-repositorio>
cd mi-proyecto-vue
```

2. **Instalar dependencias**

```bash
npm install
```

## Ejecución del proyecto

Para ejecutar la aplicación, necesitas iniciar tanto el servidor de desarrollo de Vite como el JSON Server que actúa como backend.

### 1. Iniciar el servidor de API (JSON Server)

Abre una terminal y ejecuta:

```bash
npm run api
```

Alternativamente, puedes ejecutar directamente:

```bash
json-server --watch db.json --port 3000
```

Esto iniciará JSON Server en el puerto 3000 y usará el archivo `db.json` como base de datos.

### 2. Iniciar el servidor de desarrollo de Vite

En otra terminal, ejecuta:

```bash
npm run dev
```

Esto iniciará el servidor de desarrollo en `http://localhost:5173` (o el puerto que Vite asigne automáticamente).

## Comandos disponibles

- `npm run dev`: Inicia el servidor de desarrollo
- `npm run build`: Compila y minimiza el proyecto para producción
- `npm run preview`: Previsualiza la versión de producción localmente
- `npm run api`: Inicia JSON Server como backend

## Estructura del proyecto

```
mi-proyecto-vue/
├── public/               # Archivos estáticos
├── src/
│   ├── assets/           # Recursos (imágenes, fuentes, etc.)
│   ├── view/             # Vistas/componentes de páginas
│   │   └── NivelesCarrera.vue # Componente principal de gestión
│   ├── App.vue           # Componente raíz
│   ├── main.ts           # Punto de entrada de la aplicación
│   └── style.css         # Estilos globales
├── db.json               # Base de datos para JSON Server
├── index.html            # Página HTML principal
├── package.json          # Configuración del proyecto y dependencias
├── tsconfig.json         # Configuración de TypeScript
└── vite.config.ts        # Configuración de Vite
```

## Funcionalidades

- Visualización de niveles de carrera académica
- Creación de nuevos niveles
- Edición de niveles existentes
- Eliminación de niveles

## Notas importantes

- Asegúrate de que el puerto 3000 esté disponible para JSON Server
- La aplicación utiliza Bootstrap 5 para los estilos y componentes UI
- JSON Server debe estar en ejecución para que las operaciones CRUD funcionen correctamente

## Solución de problemas

### Error: "EADDRINUSE: address already in use :::3000"

Si el puerto 3000 ya está en uso, puedes especificar otro puerto para JSON Server:

```bash
json-server --watch db.json --port 3001
```

*Recuerda actualizar la URL de la API en `src/view/NivelesCarrera.vue` si cambias el puerto.*
