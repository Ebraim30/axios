<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
import Loading from 'vue-loading-overlay'
import useGenreStore from '@/stores/genre'
import { useRouter } from 'vue-router'

const genreStore = useGenreStore()
const router = useRouter()
const isLoading = ref(false)
const genres = ref([])
const movies = ref([])

const getGenreName = (id) => genres.value.find((genre) => genre.id === id)?.name || 'Desconhecido'
const formatDate = (date) => new Date(date).toLocaleDateString('pt-BR')

const listMovies = async (genreId) => {
  genreStore.setCurrentGenreId(genreId)
  isLoading.value = true
  const response = await api.get('discover/movie', {
    params: {
      with_genres: genreId,
      language: 'pt-BR'
    }
  })
  movies.value = response.data.results
  isLoading.value = false
}

function openMovie(movieId) {
  router.push({ name: 'MovieDetails', params: { movieId } })
}

onMounted(async () => {
  isLoading.value = true
  await genreStore.getAllGenres('movie')
  isLoading.value = false
})
</script>

<template>
  <h1 class="page-title">Filmes</h1>
  <ul class="genre-list">
    <li
      v-for="genre in genreStore.genres"
      :key="genre.id"
      @click="listMovies(genre.id)"
      class="genre-item"
    >
      {{ genre.name }}
    </li>
  </ul>
  <loading v-model:active="isLoading" is-full-page />
  <div class="movie-list">
    <div v-for="movie in movies" :key="movie.id" class="movie-card">
      <img
        :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
        :alt="movie.title"
        @click="openMovie(movie.id)"
      />
      <div class="movie-details">
        <p class="movie-title">{{ movie.title }}</p>
        <p class="movie-release-date">{{ formatDate(movie.release_date) }}</p>
        <p class="movie-genres">
          <span
            v-for="genre_id in movie.genre_ids"
            :key="genre_id"
            @click="listMovies(genre_id)"
            :class="{ active: genre_id === genreStore.currentGenreId }"
          >
            {{ genreStore.getGenreName(genre_id) }}
          </span>
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Estilos Globais */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Título da Página */
.page-title {
  text-align: center;
  font-size: 2.5rem;
  font-weight: 700;
  color: #2c3e50;
  margin-top: 120px;
  margin-bottom: 40px;
  font-family: 'Arial', sans-serif;
}

/* Lista de Gêneros */
.genre-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
  padding: 0 2rem;
}

.genre-item {
  background-color: #1e90ff; /* Azul forte */
  padding: 0.6rem 1.2rem;
  border-radius: 20px;
  color: #fff;
  font-size: 0.95rem;
  font-weight: bold;
  cursor: pointer;
  text-align: center;
  transition:
    background-color 0.3s ease,
    transform 0.2s ease;
}

.genre-item:hover {
  background-color: #4682b4; /* Azul escuro */
  transform: scale(1.05);
}

.genre-item.active {
  background-color: #32cd32; /* Verde limão para gênero ativo */
}

/* Lista de Filmes */
.movie-list {
  display: grid;
  grid-template-columns: repeat(
    auto-fill,
    minmax(400px, 1fr)
  ); /* Ajustado para 200px como os programas de TV */
  gap: 1.2rem;
  padding: 0 1.5rem;
  justify-content: center;
}

.movie-card {
  width: 100%;
  height: 45rem; /* Tamanho similar aos Programas de TV */
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.3s ease;
}

.movie-card:hover {
  transform: scale(1.03);
}

.movie-poster {
  width: 50%;
  height: 50%; /* Imagem ocupa 50% do cartão */
  border-radius: 12px;
}

.movie-details {
  padding: 0.6rem;
  text-align: center;
}

.movie-title {
  font-size: 1rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 0.5rem;
}

.movie-release-date {
  font-size: 0.8rem;
  color: #777;
}

.movie-genres {
  display: flex;
  gap: 0.3rem;
  justify-content: center;
}

.movie-genres span {
  background-color: #ff6347; /* Tom de laranja (Tomate) */
  border-radius: 18px;
  padding: 0.3rem 0.7rem;
  color: #fff;
  font-size: 0.8rem;
  font-weight: bold;
  cursor: pointer;
  transition:
    background-color 0.3s ease,
    transform 0.2s ease;
}

.movie-genres span:hover {
  background-color: #e74c3c;
  transform: scale(1.05);
}

.movie-genres span.active {
  background-color: #ff4500;
}
</style>
