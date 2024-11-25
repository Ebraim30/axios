<template>
  <div class="movie-details-container">
    <div class="movie-header">
      <img
        :src="`https://image.tmdb.org/t/p/w500${movieStore.currentMovie.poster_path}`"
        :alt="movieStore.currentMovie.title"
        class="movie-poster"
      />
      <div class="movie-info">
        <h1>{{ movieStore.currentMovie.title }}</h1>
        <p class="tagline">{{ movieStore.currentMovie.tagline }}</p>
        <p class="overview">{{ movieStore.currentMovie.overview }}</p>
        <p><strong>Orçamento:</strong> ${{ movieStore.currentMovie.budget.toLocaleString() }}</p>
        <p><strong>Avaliação:</strong> {{ movieStore.currentMovie.vote_average }}/10</p>
      </div>
    </div>

    <section class="production-companies">
      <h2>Produtoras</h2>
      <div class="companies">
        <template
          v-for="company in movieStore.currentMovie.production_companies"
          :key="company.id"
        >
          <img
            v-if="company.logo_path"
            :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`"
            :alt="company.name"
            class="company-logo"
          />
          <p v-else>{{ company.name }}</p>
        </template>
      </div>
    </section>
  </div>
</template>

<script setup>
import { defineProps, onMounted } from 'vue';
import { useMovieStore } from '@/stores/movie';
const movieStore = useMovieStore();

const props = defineProps({
  movieId: {
    type: Number,
    required: true,
  },
});

onMounted(async () => {
  await movieStore.getMovieDetail(props.movieId);
});
</script>

<style scoped>
.movie-details-container {
  padding: 2rem;
  background-color: #f7f7f7;
}

.movie-header {
  display: flex;
  gap: 2rem;
  margin-bottom: 2rem;
}

.movie-poster {
  width: 250px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

.movie-info {
  max-width: 700px;
}

.movie-info h1 {
  font-size: 2.5rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.tagline {
  font-style: italic;
  color: #555;
  margin-bottom: 1rem;
}

.overview {
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.production-companies {
  margin-top: 3rem;
}

.companies {
  display: flex;
  flex-wrap: wrap;
  gap: 1.5rem;
  justify-content: start;
}

.company-logo {
  max-width: 120px;
  margin-bottom: 1rem;
}

.company-logo:hover {
  transform: scale(1.05);
  transition: transform 0.3s ease;
}

.production-companies h2 {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 1rem;
}
</style>
