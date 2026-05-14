<script setup>
import axios from 'axios';
import { onMounted } from 'vue';

onMounted(() => {
    axios
    .get('https://restcountries.com/v3.1/all?fields=name,capital,currencies'
    .then(response => this.info = response)
)
})



</script>

<template>
    <h1>This is my hello world component</h1>
</template>

<style scoped>

h1 {
    color: red;
}

</style>