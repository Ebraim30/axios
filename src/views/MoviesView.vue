<template>
  <h1 class="page-title">Filmes</h1>
  <ul class="genre-list">
    <li
      v-for="genre in genreStore.genres"
      :key="genre.id"
      @click="listMovies(genre.id)"
      class="genre-item"
      :class="{ active: genre.id === genreStore.currentGenreId }"
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
        class="movie-image"
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

<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
import Loading from 'vue-loading-overlay'
import genreStore from '@/stores/genre'
import { useRouter } from 'vue-router'

const router = useRouter()
const isLoading = ref(false)
const movies = ref([])

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
  listMovies(28)
  isLoading.value = false
})
</script>

<style scoped>
/* Geral */
.page-title {
  text-align: center;
  font-family: 'Rick and Morty', sans-serif;
  font-size: 4rem;
  color: #32bba7;
  text-shadow: 0 0 15px #32bba7, 0 0 25px #00ff00, 0 0 35px #00ff00;
  margin-bottom: 3rem;
  animation: text-flicker 2s infinite ease-in-out, glowing 1.5s ease-in-out infinite;
}

@keyframes text-flicker {
  0%, 50%, 100% {
    opacity: 1;
  }
  25%, 75% {
    opacity: 0.3;
  }
}

@keyframes glowing {
  0% {
    text-shadow: 0 0 5px #32bba7, 0 0 10px #00ff00, 0 0 15px #00ff00;
  }
  50% {
    text-shadow: 0 0 10px #32bba7, 0 0 20px #00ff00, 0 0 30px #00ff00;
  }
  100% {
    text-shadow: 0 0 5px #32bba7, 0 0 10px #00ff00, 0 0 15px #00ff00;
  }
}

/* Lista de Gêneros */
.genre-list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 2rem;
  list-style: none;
  margin-bottom: 3rem;
}

.genre-item {
  border-radius: 2rem;
  padding: 1rem 2rem;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out, background-color 0.3s ease-in-out;
  position: relative;
  overflow: hidden;
  animation: portalEffect 3s ease-in-out infinite;
  background-image: url('./public/portalzinho.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  animation: rotateEffect 10s linear infinite;
  display: flex;
  align-items: center;
  justify-content: center;
  transform-origin: center;
  animation: rotateGenreText 20s linear infinite;
}

@keyframes rotateEffect {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes rotateGenreText {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes portalEffect {
  0%, 100% {
    transform: scale(1);
    box-shadow: 0 0 2rem #00ff00;
    opacity: 0.8;
  }
  50% {
    transform: scale(1.15);
    box-shadow: 0 0 5rem #00ff00;
    opacity: 1;
  }
}

.genre-item:hover {
  background-color: #00ff00;
  transform: scale(1.1);
  box-shadow: 0 0 2rem #00ff00;
  animation: pulse 1.5s ease-in-out infinite;
}

.genre-item.active {
  background-color: #00ff00;
  font-weight: bolder;
}

/* Animação de Pulsar */
@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* Card de Filmes */
.movie-list {
  display: flex;
  flex-wrap: wrap;
  gap: 3rem;
  justify-content: center;
  margin-top: 2rem;
}

.movie-card {
  width: 18rem;
  height: 36rem;
  border-radius: 1.5rem;
  overflow: hidden;
  box-shadow: 0 0 4rem rgba(0, 0, 0, 0.8);
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out, opacity 0.3s ease;
  position: relative;
  opacity: 0.9;
  background-color: transparent;
}

.movie-card:hover {
  transform: translateY(-20px) scale(1.05);
  opacity: 1;
}

.movie-card img {
  width: 100%;
  height: 22rem;
  border-radius: 1.5rem;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  animation: rotateImage 20s infinite linear;
}

@keyframes rotateImage {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.movie-card img:hover {
  transform: scale(1.1);
  box-shadow: 0 0 2rem rgb(43, 255, 0);
}

.movie-details {
  padding: 1rem;
  background-color: transparent;
  border-radius: 0 0 1.5rem 1.5rem;
  color: #fff;
  transition: background-color 0.3s ease-in-out;
}

.movie-title {
  font-size: 1.5rem;
  font-weight: bold;
  line-height: 1.6rem;
  margin-bottom: 1rem;
  transition: color 0.3s ease;
}

.movie-title:hover {
  color: #1eff00;
}

.movie-release-date {
  font-size: 1.2rem;
  color: #ffbb33;
  margin-bottom: 0.5rem;
}

.movie-genres {
  display: flex;
  flex-wrap: wrap;
  gap: 0.8rem;
  justify-content: center;
  margin-top: 1rem;
}

.movie-genres span {
  background-color: #6200ff;
  border-radius: 1rem;
  padding: 0.4rem 1rem;
  color: #fff;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease, background-color 0.3s ease;
}

.movie-genres span:hover {
  background-color: #6200ff;
  transform: scale(1.1);
  box-shadow: 0 0 2rem rgba(255, 165, 0, 0.8);
}

.movie-genres span.active {
  background-color: #19ff04;
}
</style>
