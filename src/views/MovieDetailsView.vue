<script setup>
import { onMounted } from 'vue';
import { useMovieStore } from '@/stores/movie';

const movieStore = useMovieStore();

const props = defineProps({
    movieId: {
        type: String,
        required: true,
    },
});

onMounted(async () => {
    await movieStore.getMovieDetail(props.movieId);
});

// Função para formatar números como valores monetários
const formatCurrency = (value) => {
    if (value) {
        return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
    }
    return 'Não disponível';
}
</script>

<template>
    <div class="main">
        <div class="content">
            <img :src="`https://image.tmdb.org/t/p/w185${movieStore.currentMovie.poster_path}`"
                :alt="movieStore.currentMovie.title" class="poster" />

            <div class="details">
                <h1 class="title">{{ movieStore.currentMovie.title }}</h1>
                <p class="tagline">{{ movieStore.currentMovie.tagline }}</p>
                <p class="overview">{{ movieStore.currentMovie.overview }}</p>
                <p class="rating">Avaliação: {{ movieStore.currentMovie.vote_average }} ⭐</p>
            </div>
        </div>

        <div class="financial-summary">
            <h2>Informações Financeiras</h2>
            <p><strong>Receita:</strong> {{ formatCurrency(50000000) }} 💸</p>
            <p class="financial-info">Orçamento: <span class="budget-value">R$ 250.000.000,00</span> 💰</p>
            <p><strong>Duração:</strong> {{ movieStore.currentMovie.runtime }} minutos</p>
        </div>

        <p class="companies-title">Produtoras</p>
        <div class="companies">
            <template v-for="company in movieStore.currentMovie.production_companies" :key="company.id">
                <img v-if="company.logo_path" :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`" :alt="company.name" class="company-logo" />
                <p v-else>{{ company.name }}</p>
            </template>
        </div>
    </div>
</template>

<style scoped>
/* Estilo geral inspirado em Rick and Morty */
body {
    background-color: #171717;
    font-family: 'Press Start 2P', cursive;
    color: #fff;
    margin: 0;
    padding: 0;
}

/* Animação de fundo com efeito de "portal" */
@keyframes portalAnimation {
  0% {
    background: radial-gradient(circle, rgba(0, 255, 0, 0.3), rgba(0, 255, 0, 0.7));
  }
  50% {
    background: radial-gradient(circle, rgba(140, 0, 255, 0.4), rgba(162, 0, 255, 0.9));
  }
  100% {
    background: radial-gradient(circle, rgba(1, 255, 1, 0.521), rgba(0, 255, 0, 0.7));
  }
}

.main {
    padding: 2rem;
    animation: portalAnimation 3s ease-in-out infinite;
}

/* Título do filme */
.title {
    font-size: 2rem;
    text-align: center;
    color: #000; /* Título em preto */
    margin-bottom: 1rem;
    font-weight: bold;
}

/* Detalhes do filme */
.details {
    background: rgba(0, 255, 0, 0.1); /* Leve fundo esverdeado */
    padding: 1.5rem;
    border-radius: 12px;
    margin-top: 2rem;
}

.tagline {
    font-size: 1.1rem;
    color: #000000; /* Tom de cinza para suavizar a tagline */
    font-style: italic;
    margin-top: 1rem;
}

.overview {
    font-size: 1rem;
    color: #ccc; /* Texto mais suave para a descrição */
    margin-top: 1rem;
}

.financial-info {
    font-size: 1.1rem;
    margin-top: 1rem;
    color: #ff9000; /* Cor padrão da frase */
    font-weight: bold;
}

.budget-value {
    color: #ffffff; /* Cor branca apenas para o número */
}


.rating {
    font-size: 1.1rem;
    color: #ffbb33; /* Cor para a avaliação */
    margin-top: 1rem;
}

/* Informações financeiras */
.financial-summary {
    background-color: #282828;
    padding: 2rem;
    margin-top: 2rem;
    border-radius: 12px;
    box-shadow: 0 0 15px rgba(0, 255, 0, 0.4);
    color: #fff;
}

.financial-summary h2 {
    font-size: 1.8rem;
    color: #32bba7;
    margin-bottom: 1.5rem;
}

.financial-summary p {
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
}

.financial-summary strong {
    font-weight: bold;
    color: #ff9000;
}

/* Produtoras */
.companies-title {
    font-size: 1.6rem;
    color: #00ff00;
    text-align: center;
    margin-top: 3rem;
}

.companies {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    justify-content: center;
}

.company-logo {
    width: 100px;
    height: 100px;
    object-fit: contain;
    border-radius: 8px;
    box-shadow: 0 0 15px rgba(0, 255, 0, 0.6);
    transition: transform 0.3s ease-in-out;
}

.company-logo:hover {
    transform: scale(1.1);
}

.companies p {
    font-size: 1.2rem;
    color: #ffffff;
    font-weight: bold;
}

/* Ajustes no poster */
.poster {
    width: 185px;
    border-radius: 10px;
    box-shadow: 0 0 15px rgba(0, 255, 0, 0.7);
    margin-top: 1.5rem;
    transition: transform 0.3s ease-in-out;
}

.poster:hover {
    transform: scale(1.05);
}
</style>
