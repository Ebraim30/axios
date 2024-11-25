<template>
  <div class="tv-view-container">
  
    <h1 class="page-title">Programas de TV</h1>

   
    <div class="genre-list">
      <div
        v-for="genre in genres"
        :key="genre.id"
        @click="listMovies(genre.id)"
        class="genre-item"
        :class="{ active: genre.id === currentGenreId }"
      >
        {{ genre.name }}
      </div>
    </div>

  
    <loading v-model:active="isLoading" is-full-page />

    <div class="movie-list">
      <div v-for="tvShow in movies" :key="tvShow.id" class="movie-card" @click="openTvShow(tvShow.id)">
        <img
          :src="`https://image.tmdb.org/t/p/w500${tvShow.poster_path}`"
          :alt="tvShow.name"
          class="movie-poster"
        />
        <div class="movie-details">
          <p class="movie-title">{{ tvShow.name }}</p>
          <p class="movie-release-date">{{ tvShow.first_air_date }}</p>
          <div class="movie-genres">
            <span
              v-for="genre_id in tvShow.genre_ids"
              :key="genre_id"
              @click="listMovies(genre_id)"
              :class="{ active: genre_id === currentGenreId }"
            >
              {{ getGenreName(genre_id) }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
import Loading from 'vue-loading-overlay'
import useGenreStore from '@/stores/genre'
import { useRouter } from 'vue-router'

const genreStore = useGenreStore();
const router = useRouter();
const isLoading = ref(false);
const genres = ref([]);
const movies = ref([]);
const currentGenreId = ref(null);

const getGenreName = (id) => genres.value.find((genre) => genre.id === id)?.name || 'Desconhecido';

const listMovies = async (genreId) => {
  currentGenreId.value = genreId;
  isLoading.value = true;
  const response = await api.get('discover/tv', {
    params: {
      with_genres: genreId,
      language: 'pt-BR',
    },
  });
  movies.value = response.data.results;
  isLoading.value = false;
};

const openTvShow = (tvShowId) => {
  router.push({ name: 'TvShowDetails', params: { tvShowId } });
};

onMounted(async () => {
  isLoading.value = true;
  const response = await api.get('genre/tv/list?language=pt-BR');
  genres.value = response.data.genres;
  isLoading.value = false;
});
</script>

<style scoped>
/* Título da Página */
.page-title {
  text-align: center;
  font-size: 3rem;
  font-weight: 700;
  color: #2c3e50;
  margin-bottom: 2rem;
}

/* Lista de Gêneros */
.genre-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
  padding: 0 2rem;
}

.genre-item {
  background-color: #3498db; /* Azul Claro */
  padding: 0.8rem 1.5rem;
  border-radius: 25px;
  color: #fff;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  text-align: center;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.genre-item:hover {
  background-color: #2980b9; /* Azul Escuro */
  transform: scale(1.05);
}

.genre-item.active {
  background-color: #1abc9c; /* Verde para gênero ativo */
}

/* Lista de Programas de TV */
.movie-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 2rem;
  padding: 0 2rem;
  justify-content: center;
}

.movie-card {
  width: 100%;
  height: 100%;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.3s ease;
}

.movie-card:hover {
  transform: scale(1.05);
}

.movie-poster {
  width: 100%;
  height: 70%;
  object-fit: cover;
  border-radius: 15px;
}

.movie-details {
  padding: 1rem;
  text-align: center;
}

.movie-title {
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 0.5rem;
}

.movie-release-date {
  font-size: 0.9rem;
  color: #777;
}

.movie-genres {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
}

.movie-genres span {
  background-color: #f39c12; /* Laranja Quente */
  border-radius: 20px;
  padding: 0.4rem 0.8rem;
  color: #fff;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.movie-genres span:hover {
  background-color: #e67e22; /* Laranja mais escuro */
  transform: scale(1.05);
}

.movie-genres span.active {
  background-color: #e74c3c; /* Vermelho para o gênero ativo */
}
</style>
