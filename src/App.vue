<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router';
import HelloWorld from './components/HelloWorld.vue';
</script>

<template>
  <div class="app">
    <!-- Cabeçalho com logo e navegação -->
    <header>
      <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/vocabulos">Gerencie Vocábulos</RouterLink>
      </nav>
    </header>

    <!-- Conteúdo principal -->
    <main>
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
/* Estilo base para a aplicação */
.app {
  /*display: flex;*/
  flex-direction: column;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  background-color: #f1fdf4; /* Fundo verde claro */
  color: #2d6a4f; /* Texto em verde escuro */
}

/* Cabeçalho */
header {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #d3f9d8; /* Verde menta claro */
  padding: 1rem;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.logo {
  margin-bottom: 1rem;
}

/* Navegação */
nav {
  display: flex;
  gap: 1.5rem;
}

nav a {
  text-decoration: none;
  font-size: 1rem;
  color: #2d6a4f; /* Verde escuro */
  padding: 0.5rem 1rem;
  border-radius: 10px;
  transition: background-color 0.3s ease;
}

nav a:hover {
  background-color: #95d5b2; /* Verde intermediário */
  color: white;
}

nav a.router-link-exact-active {
  background-color: #52b788; /* Verde escuro ativo */
  color: white;
}

/* Conteúdo principal */
main {
  flex: 1;
  padding: 2rem;
  background-color: #f1fdf4; /* Fundo verde claro */
  box-shadow: inset 0px 2px 10px rgba(0, 0, 0, 0.05);
  border-radius: 10px;
}
</style>
