<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
import Loading from 'vue-loading-overlay'
import genreStore from '@/stores/genre'
import { useRouter } from 'vue-router'

const router = useRouter()
const isLoading = ref(false)
const tvs = ref([])

const formatDate = (date) => new Date(date).toLocaleDateString('pt-BR')

const listTvs = async (genreId) => {
  genreStore.setCurrentGenreId(genreId)
  isLoading.value = true
  const response = await api.get('discover/tv', {
    params: {
      with_genres: genreId,
      language: 'pt-BR'
    }
  })
  tvs.value = response.data.results
  isLoading.value = false
}

function openTv(tvId) {
  router.push({ name: 'SeriesDetails', params: { tvId } })
}

onMounted(async () => {
  isLoading.value = true
  await genreStore.getAllGenres('tv')
  
  // Encontrar o gênero "Soap" pelo nome e definí-lo como o gênero atual
  const soapGenre = genreStore.genres.find(genre => genre.name.toLowerCase() === 'soap')
  
  if (soapGenre) {
    genreStore.setCurrentGenreId(soapGenre.id)
    await listTvs(soapGenre.id)  // Carregar as séries do gênero "Soap"
  }

  isLoading.value = false
})

</script>

<template>
  <h1 class="page-title">Séries</h1>
  <ul class="genre-list">
    <li
      v-for="genre in genreStore.genres"
      :key="genre.id"
      @click="listTvs(genre.id)"
      class="genre-item"
      :class="{ active: genre.id === genreStore.currentGenreId }"
    >
      {{ genre.name }}
    </li>
  </ul>

  <loading v-model:active="isLoading" is-full-page />

  <div class="tv-list">
    <div v-for="tv in tvs" :key="tv.id" class="tv-card">
      <img
        :src="`https://image.tmdb.org/t/p/w500${tv.poster_path}`"
        :alt="tv.name"
        @click="openTv(tv.id)"
        class="tv-image"
      />
      <div class="tv-details">
        <p class="tv-title">{{ tv.name }}</p>
        <p class="tv-first-air-date">{{ formatDate(tv.first_air_date) }}</p>
        <p class="tv-genres">
          <span
            v-for="genre_id in tv.genre_ids"
            :key="genre_id"
            @click="listTvs(genre_id)"
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
  background-color: #32bba7;
  border-radius: 2rem;
  padding: 1rem 2rem;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out, background-color 0.3s ease-in-out;
  position: relative;
  overflow: hidden;
  animation: portalEffect 3s ease-in-out infinite; /* Animação de efeito de portal */
  background-image: url('./public/portalzinho.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  animation: rotateEffect 10s linear infinite; /* Aumento do tempo de rotação */
  display: flex;
  align-items: center;
  justify-content: center;
  transform-origin: center;
  animation: rotateGenreText 20s linear infinite; /* Aumento do tempo de rotação */
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

/* Card de Séries */
.tv-list {
  display: flex;
  flex-wrap: wrap;
  gap: 3rem;
  justify-content: center;
  margin-top: 2rem;
}

.tv-card {
  width: 18rem;
  height: 36rem;
  border-radius: 1.5rem;
  overflow: hidden;
  box-shadow: 0 0 4rem rgba(0, 0, 0, 0.8);
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out, opacity 0.3s ease;
  position: relative;
  opacity: 0.9;
  background-color: #000; /* Adicionando fundo preto para combinar com o estilo do movie-card */
}

.tv-card:hover {
  transform: translateY(-20px) scale(1.05);
  box-shadow: 0 0 5rem rgba(0, 0, 0, 0.8);
  opacity: 1;
}

.tv-card img {
  width: 100%;
  height: 22rem;
  border-radius: 1.5rem;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  object-fit: cover; /* Garantir que a imagem cubra todo o espaço do card */
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

.tv-card img:hover {
  transform: scale(1.1);
  box-shadow: 0 0 2rem rgb(1, 255, 14);
}

.tv-details {
  padding: 1rem;
  background-color: rgba(0, 0, 0, 0.85);
  border-radius: 0 0 1.5rem 1.5rem;
  color: #fff;
  transition: background-color 0.3s ease-in-out;
}

.tv-title {
  font-size: 1.5rem;
  font-weight: bold;
  line-height: 1.6rem;
  margin-bottom: 1rem;
  transition: transform 0.3s ease;
}

.tv-title:hover {
  transform: scale(1.05);
  color: #00ff00;
}

.tv-first-air-date {
  font-size: 1.2rem;
  color: #ffbb33;
  margin-bottom: 0.5rem;
}

.tv-genres {
  display: flex;
  flex-wrap: wrap;
  gap: 0.8rem;
  justify-content: center;
  margin-top: 1rem;
}

.tv-genres span {
  background-color: #6200ff;
  border-radius: 1rem;
  padding: 0.4rem 1rem;
  color: #fff;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.tv-genres span:hover {
  background-color: #6200ff;
  transform: scale(1.1);
  box-shadow: 0 0 1rem rgba(255, 165, 0, 0.8);
}

.tv-genres span.active {
  background-color: #00ff00;
  color: #000;
  font-weight: bolder;
}

@media (max-width: 768px) {
  .page-title {
    font-size: 2.5rem;
  }

  .tv-card {
    width: 14rem;
    height: 28rem;
  }

  .tv-title {
    font-size: 1.2rem;
  }

  .tv-first-air-date {
    font-size: 1rem;
  }

  .tv-genres span {
    font-size: 1rem;
  }
}
</style>
