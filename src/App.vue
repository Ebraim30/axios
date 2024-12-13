<template>
  <div>
    <header>
      <nav>
        <router-link to="/">Home</router-link>
        <router-link to="/filmes">Filmes</router-link>
        <router-link to="/tv">Programas de TV</router-link>
      </nav>
    </header>

    <main>
      <router-view />
    </main>
  </div>
</template>

<style scoped>
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #252525;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

nav {
  display: flex;
  gap: 2rem;
}

nav a {
  text-decoration: none;
  color: #ffffff;
  font-size: 1.2rem;
  font-weight: bold;
  transition: color 0.3s ease;
  position: relative;
  transition: transform 0.8s ease;
}

nav a:hover {
  color: #f10f0f;
  transform: translateX(6px);
  animation: 0.2s;
}

main {
  margin-top: 4rem;
}
</style>
