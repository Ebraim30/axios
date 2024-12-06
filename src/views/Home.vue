<template>
  <div class="home-container">
    <section class="hero">
      <div class="hero-overlay"></div>
      <div class="hero-content">
        <h1>Bem-vindo à Cinemania!</h1>
        <p>
          Explore filmes e séries incríveis. Encontre seus favoritos, descubra novos conteúdos e
          fique por dentro de todas as novidades!
        </p>
        <button @click="scrollToSignup" class="cta-button">Cadastre-se Agora</button>
      </div>
    </section>

    <section id="signup" class="signup-section">
      <h2>Junte-se à nossa comunidade</h2>
      <p>
        Receba atualizações exclusivas sobre os filmes e programas mais recentes. Inscreva-se para
        não perder nada!
      </p>
      <form @submit.prevent="submitSignup" class="signup-form">
        <input v-model="email" type="email" placeholder="Digite seu e-mail" required />
        <button type="submit">Cadastrar</button>
      </form>
    </section>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const email = ref('')

const submitSignup = () => {
  if (email.value) {
    alert(`Obrigado por se inscrever com o e-mail: ${email.value}`)
    email.value = '' // Limpar o campo de e-mail após a inscrição
  } else {
    alert('Por favor, insira um e-mail válido!')
  }
}

const scrollToSignup = () => {
  const signupSection = document.getElementById('signup')
  signupSection.scrollIntoView({ behavior: 'smooth' })
}
</script>

<style scoped>
.home-container {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.hero {
  position: relative;
  background-image: url('src/assets/img/53d8d059-a7d4-426b-8057-dbf35f835437.webp');
  background-size: cover;
  background-position: center;
  color: white;
  text-align: center;
  padding: 200px 20px;
  height: 100vh;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5); /* Overlay para melhorar a legibilidade do texto */
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
}

.hero-content p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
}

.cta-button {
  padding: 1rem 2.5rem;
  background-color: #fc4323;
  color: white;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: background-color 0.3s ease;
}

.cta-button:hover {
  background-color: #e55347;
}

.signup-section {
  background-color: #f7f7f7;
  text-align: center;
  padding: 60px 20px;
}

.signup-section h2 {
  font-size: 2.8rem;
  margin-bottom: 1.5rem;
}

.signup-section p {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
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
  border: 1px solid #ddd;
  width: 100%;
  max-width: 350px;
  margin-bottom: 1rem;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

.signup-form input:focus {
  border-color: #ff6347;
  outline: none;
}

.signup-form button {
  padding: 1rem 2.5rem;
  background-color: #387250;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1.2rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.signup-form button:hover {
  background-color: #4e9e5f;
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
