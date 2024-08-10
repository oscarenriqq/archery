<template>
    <div>
        <form @submit="handleSubmit" class="flex flex-col items-center">
            <p class="md:text-xl font-extrabold mt-10"> Pista: {{ question.pista }} </p>
            <div class="w-1/2 flex justify-between mt-5 px-1">
                <span class="font-extrabold">La palabra tiene {{ `${question.palabra.length} letras` }}</span>
                <span v-if="answer.length > 0" class="font-extrabold">{{ answer.length }}</span>
            </div>
            <input 
                type="text"
                v-model="answer"
                class="w-1/2 h-10 border-2 rounded-md outline-none text-center font-extrabold"
                placeholder="Ingresa tu respuesta"
                required
            />
            <span class="font-extrabold mt-2">{{ hits }} / {{ hitsToWin }}</span>
            <button type="submit" class="mt-5 bg-black w-40 h-10 rounded-lg text-white font-extrabold"> Verificar </button>
        </form>
    </div>
</template>

<script setup>
import { onBeforeMount, ref, defineEmits, defineProps } from 'vue'

import data from '@/data/questions.json'

const emit = defineEmits([
    'missedAttempt',
    'incrementHit'
])

const props = defineProps([
    'hits',
    'hitsToWin'
])

const question = ref({})
const answer = ref('')
const prevAnswers = ref([])

function getQuestion(data) {

    answer.value = ''

    const indexQuestion = generateRandomIndex(data.length)
    question.value = data[indexQuestion]
    prevAnswers.value.push(indexQuestion)
}

function generateRandomIndex(max) {
    let index = Math.floor(Math.random() * max)
    if (prevAnswers.value.includes(index))
        generateRandomIndex(max)
    return index
}

function handleSubmit(e) {
    e.preventDefault()

    if (answer.value.toLowerCase() == question.value.palabra.toLowerCase()) {
        emit('incrementHit')
        getQuestion(data)
    }
    else {
        answer.value = ''
        emit('missedAttempt')
    }
}

onBeforeMount(() => {
    getQuestion(data)
})

</script>