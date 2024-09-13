# Generador Automático de Películas

## Descripción
El Generador Automático de Películas es una aplicación web desarrollada con Vue.js que permite a los usuarios descubrir películas de forma aleatoria basándose en criterios como género, puntuación mínima y rango de años donde fue estrernada la película. La aplicación utiliza la API de The Movie Database (TMDB) para obtener información sobre películas y géneros.

## Características
- Selección de género de película, basadas en las que provee TMDB en su API
- Filtro de puntuación mínima (1-5 estrellas)
- Selección de rango de años para las películas
- Búsqueda aleatoria de películas basada en los criterios seleccionados
- Visualización de detalles de la película incluyendo póster, título, sinopsis, fecha de estreno y puntuación

## Tecnologías Utilizadas
- Vue.js 3 (Composition API)
- Vite (build tool)
- Tailwind CSS (estilos)
- Lucide Vue Next (iconos)
- TMDB API

## Requisitos Previos
- Node.js (versión 14 o superior)
- NPM o Yarn
- Una clave de API de TMDB

## Instalación

1. Clona este repositorio:
   ```
   git clone https://github.com/davidfl214/random-movie-generator
   ```

2. Navega al directorio del proyecto:
   ```
   cd random-movie-generator
   ```

3. Instala las dependencias:
   ```
   npm install
   ```
   o si usas Yarn:
   ```
   yarn install
   ```

4. Crea un archivo `.env` en la raíz del proyecto y añade tu clave de API de TMDB:
   ```
   VITE_TMDB_API_KEY=tu_clave_api_aqui
   ```

## Uso

1. Inicia el servidor de desarrollo:
   ```
   npm run dev
   ```
   o con Yarn:
   ```
   yarn dev
   ```

2. Abre tu navegador y visita `http://localhost:5173` (o el puerto que Vite te indique).

3. Usa la interfaz para seleccionar tus criterios de búsqueda y haz clic en "Buscar película" para obtener una recomendación aleatoria.

## Construcción para Producción

Para construir la aplicación para producción, ejecuta:

```
npm run build
```
o con Yarn:
```
yarn build
```

Esto generará una versión optimizada de la aplicación en el directorio `dist`.

## Contribuir
Las contribuciones son bienvenidas. Por favor, abre un issue para discutir cambios mayores antes de crear un pull request.

Link del proyecto: [https://github.com/davidfl214/random-movie-generator](https://github.com/davidfl214/random-movie-generator)