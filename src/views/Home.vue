<template>
  <div class="home-container">
    <section class="hero">
      <div class="hero-overlay"></div>
      <div class="hero-content">
        <h1>Bem-vindo ao Multiverso de Rick and Morty!</h1>
        <p>
          Explore aventuras interdimensionais, teorias malucas e mergulhe no caos do multiverso. Faça parte da nossa comunidade!
        </p>
        <button @click="goToFilmes" class="cta-button">Entrar no Portal</button>
      </div>
    </section>

    <section id="signup" class="signup-section">
      <h2>Junte-se à Sociedade Interdimensional</h2>
      <p>
        Receba atualizações sobre aventuras épicas, episódios incríveis e teorias interdimensionais. Inscreva-se agora!
      </p>
      <form @submit.prevent="submitSignup" class="signup-form">
        <input v-model="email" type="email" placeholder="Digite seu e-mail" required />
        <button type="submit" class="cdst_button"></button> <!-- Apenas o botão sem texto -->
      </form>
    </section>

    <!-- Modal de sucesso -->
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h2 class="modal-title">Obrigado, terráqueo!</h2>
        <p class="modal-message">Você se inscreveu com: <strong>{{ email }}</strong></p>
        <button @click="closeModal" class="modal-close-btn">Fechar</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const email = ref('')
const router = useRouter()
const showModal = ref(false)

const submitSignup = () => {
  if (email.value) {
    showModal.value = true
  } else {
    alert('Ei, não seja um Jerry! Insira um e-mail válido!')
  }
}

const goToFilmes = () => {
  router.push('/filmes') // Redireciona para a página /filmes
}

const closeModal = () => {
  showModal.value = false
  email.value = '' // Limpar o campo de e-mail após a inscrição
}
</script>

<style scoped>
.cdst_button {
  background-image: url('./public/portalzada.png');
  background-size: 200% 200%; /* Aumenta o zoom na imagem para mostrar apenas o portal e dedo do meio */
  background-position: center; /* Centraliza a parte do portal e dedo do meio */
  width: 80px; /* Ajuste do tamanho do botão */
  height: 80px; /* Ajuste do tamanho do botão */
  border-radius: 50%; /* Torna o botão redondo */
  border: none;
  cursor: pointer;
  transition: transform 0.3s ease, filter 0.3s ease;
  animation: portal-animation 2s infinite ease-in-out; /* Animação de portal */
}

/* Animação para simular uma distorção de portal */
@keyframes portal-animation {
  0% {
    transform: rotate(0deg) scale(1);
    filter: brightness(1) saturate(1);
  }
  50% {
    transform: rotate(360deg) scale(1.1); /* Rotaciona e aumenta o botão */
    filter: brightness(1.5) saturate(1.5); /* Aumenta o brilho e saturação */
  }
  100% {
    transform: rotate(720deg) scale(1);
    filter: brightness(1) saturate(1);
  }
}

.cdst_button:hover {
  transform: scale(1.1); /* Leve aumento do botão ao passar o mouse */
}

.home-container {
  font-family: 'Rick and Morty', 'Arial', sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.hero {
  position: relative;
  background-image: url('./public/rickzada.png');
  background-size: cover;
  background-position: center;
  color: #00ff00;
  text-align: center;
  padding: 200px 20px;
  height: 100vh;
  margin-top: -2px;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  z-index: 0;
}

.hero-content {
  position: relative;
  z-index: 1;
}

.hero-content h1 {
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  text-shadow: 0 0 10px #00ff00;
}

.hero-content p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  color: #ffffff;
}

.cta-button {
  padding: 1rem 2.5rem;
  background-color: #00ff00;
  color: #000000;
  border: 2px solid #ffffff;
  border-radius: 50px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: transform 0.8s ease, background-color 0.3s ease;
  transform: scale(1);
  font-weight: bold;
}

.cta-button:hover {
  background-color: #33ff33;
  transform: scale(1.1);
}

.signup-section {
  background-color: #1e1e1e;
  color: #00ff00;
  text-align: center;
  padding: 60px 20px;
}

.signup-section h2 {
  font-size: 2.8rem;
  margin-bottom: 1.5rem;
  text-shadow: 0 0 8px #33ff33;
}

.signup-section p {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  color: #ffffff;
}

.signup-form {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.signup-form input {
  padding: 1rem;
  font-size: 1.1rem;
  border-radius: 5px;
  border: 1px solid #33ff33;
  width: 100%;
  max-width: 350px;
  margin-bottom: 1rem;
  box-sizing: border-box;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.signup-form input:focus {
  border-color: #00ff00;
  outline: none;
  box-shadow: 0 0 10px #00ff00;
}

.signup-form button {
  padding: 1rem 2.5rem;
  background-color: #ff9000;
  color: #000000;
  border: none;
  border-radius: 50px;
  font-size: 1.2rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.signup-form button:hover {
  background-color: #ffaa33;
}

/* Estilos do Modal */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background-color: #1e272e;
  color: #00ff00;
  padding: 30px 50px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 255, 0, 0.5);
  animation: modal-appear 0.5s ease-out;
}

.modal-title {
  font-size: 2rem;
  font-weight: bold;
  text-shadow: 0 0 5px #00ff00;
}

.modal-message {
  font-size: 1.2rem;
  margin: 15px 0;
}

.modal-close-btn {
  padding: 0.8rem 2rem;
  background-color: #ff0000;
  color: #fff;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: background-color 0.3s ease;
}

.modal-close-btn:hover {
  background-color: #ff3333;
}

@keyframes modal-appear {
  0% { opacity: 0; transform: scale(0.9); }
  100% { opacity: 1; transform: scale(1); }
}

@media (max-width: 768px) {
  .hero-content h1 {
    font-size: 2.5rem;
  }

  .hero-content p {
    font-size: 1rem;
  }

  .cta-button {
    font-size: 1rem;
    padding: 0.8rem 2rem;
  }

  .signup-section h2 {
    font-size: 2.2rem;
  }
}
</style>
