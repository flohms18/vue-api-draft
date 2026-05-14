<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

const info = ref([])

onMounted(() => {
    axios
        .get('https://restcountries.com/v3.1/name/peru')
        .then(response => { info.value = response.data })
})

</script>

<template>
    {{  info }}
</template>

<style scoped>

h1 {
    color: red;
}

</style>