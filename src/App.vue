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
  background-color: #333;
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
  color: #fff;
  font-size: 1.2rem;
  font-weight: bold;
  transition: color 0.3s ease;
}

nav a:hover {
  color: #f1c40f;
}

main {
  margin-top: 4rem;
}
</style>
