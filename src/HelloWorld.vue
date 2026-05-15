<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';

const info = ref([])
const toto = ref([])

const jackdaw = ref('James')

onMounted(() => {
    axios
        .get('https://restcountries.com/v3.1/all?fields=name')
        .then(response => { info.value = response.data })
        .then(() => console.log(info.value))
        .then(() => toto.value = info.value)
        .catch(error => console.log(error))

    
})

</script>

<template>
    {{  toto }}

    {{ jackdaw }}
</template>

<style scoped>

h1 {
    color: red;
}

</style>