<script setup>

import Header from './Header.vue';
import Footer from './Footer.vue';
import HelloWorld from './ChatComponent.vue';
</script>

<template>
  <Header></Header>
  <HelloWorld></HelloWorld>
  <Footer></Footer>

  
  
</template>

<style>
:root {
  --dark-color: #0c0116;
  --violet-color: #5c6bff;
  --whitey-color: #F8F9FA;
}

@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Light.ttf') format('truetype');
  font-weight: 300;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Regular.ttf') format('truetype');
  font-weight: 400;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Medium.ttf') format('truetype');
  font-weight: 500;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-SemiBold.ttf') format('truetype');
  font-weight: 600;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Bold.ttf') format('truetype');
  font-weight: 700;
}

* {
  font-family: 'Space Grotesk', sans-serif;
}
</style>
