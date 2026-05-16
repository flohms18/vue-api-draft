<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import ollama from 'ollama'

const response = await ollama.chat({
    model: 'llama 3.1',
     messages: [{ 
        role: 'user', 
        content: 'Why is the sky blue?' 
    }],

})

const intro = ref('Albus is a web app that help you understand all concepts of data & AI. From Governance Framework to assimilate LLM technology, Engage the conversation with Albus!')




</script>

<template>

<section class="d-flex justify-content-center align-items-center py-4 px-3">
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-12 col-md-8 col-lg-6 text-center">
                <h2 class="chat-title-text">Learn from Albus</h2>
            </div>
        </div>
        <div class="row justify-content-center">
            <div class="col-12 col-md-8 col-lg-6 text-center">
                {{ intro }}
            </div>
        </div>
    </div>
</section>
   
</template>

<style scoped>

</style>