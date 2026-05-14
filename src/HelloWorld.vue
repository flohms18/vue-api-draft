<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

const info = ref([])

const jackdaw = ref('James')

onMounted(() => {
    axios
        .get('https://restcountries.com/v3.1/all?fields=name,capital,currencies')
        .then(response => { info.value = response.data })

    jackdaw.value = 'Bond'

    
})

</script>

<template>
    {{  info }}

    {{ jackdaw }}
</template>

<style scoped>

h1 {
    color: red;
}

</style>