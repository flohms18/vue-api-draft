<script setup>
import { ref, nextTick } from 'vue';
import ollama from 'ollama';

const intro = ref('Albus is a web app that help you understand all concepts of data & AI. From Governance Framework to assimilate LLM technology, Engage the conversation with Albus!')

const messages = ref([]);
const userInput = ref('');
const isLoading = ref(false);
const chatWindow = ref(null);

async function sendMessage() {
    const content = userInput.value.trim();
    if (!content || isLoading.value) return;

    messages.value.push({ role: 'user', content });
    userInput.value = '';
    isLoading.value = true;

    await nextTick();
    scrollToBottom();

    try {
        const res = await ollama.chat({
            model: 'gemma3:270m',
            messages: messages.value.map(m => ({ role: m.role, content: m.content })),
        });
        messages.value.push({ role: 'assistant', content: res.message.content });
    } catch {
        messages.value.push({ role: 'assistant', content: 'Sorry, something went wrong. Make sure Ollama is running.' });
    } finally {
        isLoading.value = false;
        await nextTick();
        scrollToBottom();
    }
}

function scrollToBottom() {
    if (chatWindow.value) {
        chatWindow.value.scrollTop = chatWindow.value.scrollHeight;
    }
}

function handleKeydown(e) {
    if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        sendMessage();
    }
}
</script>

<template>
<section class="chat-page">

    <div class="chat-header">
        <h2 class="chat-title-text">Learn from Albus</h2>
        <p class="intro-text">{{ intro }}</p>
    </div>

    <div class="chat-window" ref="chatWindow">
        <div v-if="messages.length === 0" class="empty-state">
            Ask Albus anything about data & AI...
        </div>
        <div
            v-for="(msg, i) in messages"
            :key="i"
            class="message-row"
            :class="msg.role"
        >
            <div class="bubble">
                <span class="sender">{{ msg.role === 'user' ? 'You' : 'Albus' }}</span>
                <p>{{ msg.content }}</p>
            </div>
        </div>
        <div v-if="isLoading" class="message-row assistant">
            <div class="bubble typing">
                <span class="dot"></span>
                <span class="dot"></span>
                <span class="dot"></span>
            </div>
        </div>
    </div>

    <div class="input-bar">
        <textarea
            v-model="userInput"
            @keydown="handleKeydown"
            placeholder="Ask Albus..."
            rows="1"
            class="chat-input"
            :disabled="isLoading"
        ></textarea>
        <button
            @click="sendMessage"
            class="send-btn"
            :disabled="isLoading || !userInput.trim()"
        >
            &#9650;
        </button>
    </div>

</section>
</template>

<style scoped>

.chat-page {
    position: fixed;
    inset: 0;
    display: flex;
    flex-direction: column;
    background-color: var(--whitey-color);
}

.chat-header {
    padding: 1.5rem 2rem 1rem;
    text-align: center;
    flex-shrink: 0;
}

.chat-title-text {
    color: var(--dark-color);
    font-weight: 700;
    margin-bottom: 0.4rem;
}

.intro-text {
    color: var(--dark-color);
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0;
}

/* Scrollable history */
.chat-window {
    flex: 1;
    overflow-y: auto;
    padding: 1.5rem 2rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    scrollbar-width: thin;
    scrollbar-color: var(--dark-color) transparent;
}

.chat-window::-webkit-scrollbar {
    width: 6px;
}
.chat-window::-webkit-scrollbar-thumb {
    background: var(--violet-color);
    border-radius: 4px;
}

.empty-state {
    color: var(--dark-color);
    text-align: center;
    margin: auto;
    font-size: 0.9rem;
}

/* Message rows */
.message-row {
    display: flex;
}

.message-row.user {
    justify-content: flex-end;
}

.message-row.assistant {
    justify-content: flex-start;
}

.bubble {
    max-width: min(75%, 720px);
    width: fit-content;
    padding: 0.75rem 1rem;
    border-radius: 14px;
    font-size: 0.9rem;
    line-height: 1.6;
    word-break: break-word;
    overflow-wrap: break-word;
}

.message-row.user .bubble {
    background-color: var(--dark-color);
    color: var(--whitey-color);
    border-bottom-right-radius: 4px;
}

.message-row.assistant .bubble {
    background-color: var(--violet-color);
    color: var(--whitey-color);
}

.sender {
    display: block;
    font-size: 0.72rem;
    font-weight: 600;
    margin-bottom: 0.25rem;
    opacity: 0.7;
    text-transform: uppercase;
    letter-spacing: 0.04em;
}

.bubble p {
    margin: 0;
    white-space: pre-wrap;
}

/* Typing indicator */
.bubble.typing {
    display: flex;
    gap: 5px;
    align-items: center;
    padding: 0.85rem 1.1rem;
}

.dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background-color: var(--violet-color);
    animation: bounce 1.2s infinite;
}

.dot:nth-child(2) { animation-delay: 0.2s; }
.dot:nth-child(3) { animation-delay: 0.4s; }

@keyframes bounce {
    0%, 80%, 100% { transform: translateY(0); opacity: 0.4; }
    40%            { transform: translateY(-6px); opacity: 1; }
}

/* Input bar */
.input-bar {
    display: flex;
    align-items: flex-end;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background-color: var(--dark-color);
}

.chat-input {
    flex: 1;
    resize: none;
    background: var(--dark-color);
    border-radius: 10px;
    color: var(--whitey-color);
    font-family: inherit;
    font-size: 0.9rem;
    padding: 0.65rem 0.9rem;
    outline: none;
    line-height: 1.5;
    max-height: 120px;
    overflow-y: auto;
    transition: border-color 0.2s;
}

.chat-input::placeholder {
    color: var(--dark-color);
}

.chat-input:focus {
    border-color: var(--dark-color);
}

.chat-input:disabled {
    opacity: 0.5;
}

.send-btn {
    background-color: var(--violet-color);
    border: none;
    border-radius: 10px;
    color: var(--whitey-color);
    cursor: pointer;
    font-size: 0.85rem;
    padding: 0.65rem 0.85rem;
    transition: background-color 0.2s, opacity 0.2s;
    line-height: 1;
    flex-shrink: 0;
}

.send-btn:hover:not(:disabled) {
    background-color: #4a59e8;
}

.send-btn:disabled {
    opacity: 0.35;
    cursor: not-allowed;
}

@media (max-width: 640px) {
    .chat-header {
        padding: 1rem 1rem 0.5rem;
    }

    .intro-text {
        font-size: 0.85rem;
    }

    .chat-window {
        padding: 1rem;
        gap: 0.75rem;
    }

    .bubble {
        max-width: 90%;
        font-size: 0.85rem;
        padding: 0.65rem 0.85rem;
    }

    .input-bar {
        padding: 0.5rem 0.75rem;
    }
}

@media (max-width: 400px) {
    .bubble {
        max-width: 95%;
    }
}
</style>
