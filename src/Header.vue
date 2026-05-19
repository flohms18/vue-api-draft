<script setup>
</script>

<template>

<nav class="navbar navbar-expand-lg">
  <a class="brand" href="#">Albus</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
</nav>
</template>

<style scoped>

:root {
    --dark-color : #0c0116;
    --violet-color : #5c6bff;
    --whitey-color : #F8F9FA
}


nav {
  background-color: var(--dark-color);
}

.brand {
  color: var(--dark-color);
  text-decoration: none;
  font-size: 1.8rem;
  padding-left: 1rem;
}

.brand:hover {
  color: var(--dark-color);
  text-decoration: none;
}
</style>