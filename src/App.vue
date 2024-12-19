<template>
  <header>
    <nav>
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/filmes" class="nav-link">Filmes</router-link>
      <router-link to="/tv" class="nav-link">Séries</router-link>
    </nav>
  </header>
  <main>
    <section>
      <router-view />  
    </section>
  </main>
</template>

<style scoped>
header {
  height: 3rem;
  display: flex;
  background-color: #1a1a1a; /* Fundo mais escuro para o efeito de espaço */
  color: #9130ff; /* Amarelo para contraste */
  font-size: 1.2rem;
  padding-left: 2rem;
  font-family: 'Get Schwifty', sans-serif;
  border-bottom: 4px solid #9130ff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2); /* Sombras sutis */
  animation: navbar-appear 1s ease-out; /* Animação para a barra de navegação */
}

nav {
  column-gap: 2rem;
  margin-bottom: 0;
  display: flex;
  align-items: center;
}

.nav-link {
  text-decoration: none;
  color: #9130ff;
  position: relative;
  padding: 0.5rem;
  font-weight: bold;
  font-size: 1.2rem;
  transition: color 0.3s ease, transform 0.3s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #9130ff;
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: #00ff00; /* Cor vibrante para o hover */
  transform: scale(1.1); /* Animação de aumento ao passar o mouse */
}

.nav-link:hover::after {
  width: 100%; /* O sublinhado se expande para a largura total */
}

section {
  min-height: 100vh;
  background: radial-gradient(circle, #000, #111);
  position: relative;
  overflow: hidden;
  background-color: #000; /* Fundo escuro do espaço */
  background-image: url('https://www.transparenttextures.com/patterns/stardust.png'); /* Imagem de fundo para as estrelas */
  animation: Gradient 10s ease-in-out infinite, starsAnimation 10s linear infinite;
}

h1 {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 3rem;
  text-transform: uppercase;
  text-align: center;
  color: #ffffff;
  font-family: 'Get Schwifty', sans-serif;
  border: 3px solid #ffdd59;
  padding: 1rem;
  background: rgba(0, 0, 0, 0.7);
  animation: floaty 3s ease-in-out infinite;
}

@keyframes Gradient {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

@keyframes starsAnimation {
  0% { background-position: 0% 0%; }
  100% { background-position: 100% 100%; }
}

@keyframes navbar-appear {
  0% { opacity: 0; transform: translateY(-10px); }
  100% { opacity: 1; transform: translateY(0); }
}

@keyframes floaty {
  0%, 100% { transform: translate(-50%, -50%) translateY(-10px); }
  50% { transform: translate(-50%, -50%) translateY(10px); }
}
</style>

<style>
::-webkit-scrollbar {
  width: 0px;
}

:root {
  --primary-bg: #000; /* Preto para simular o espaço */
  --secondary-bg: #111; /* Preto mais suave para o fundo */
  --primary-text: #ffffff;
  --secondary-text: #a4b0be;
  --accent-color: #ffdd59; /* Amarelo para destaque */
  --hover-color: #00ff00; /* Cor para hover */
}

body {
  font-family: 'Get Schwifty', sans-serif;
  background-color: var(--primary-bg);
  color: var(--primary-text);
  margin: 0;
  padding: 0;
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}








</style>

<link href="https://fonts.googleapis.com/css2?family=Get+Schwifty&display=swap" rel="stylesheet">
