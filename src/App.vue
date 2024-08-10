<template>
  <div class="w-full max-w-screen-lg mx-auto">
    <header class="mt-10 flex flex-col items-center">
      <img :src="ArcheryLogo" class="w-20 h-20 mx-auto" />
      <h1 class="text-6xl font-extrabold text-center">Super arquero</h1>
      <p class="px-5 md:px-0 mt-3">Demuestra tu puntería respondiendo las preguntas y salva a un inocente civil de morir.</p>
    </header>
    <main class="text-center pt-10">
      <div class="flex justify-between">
        <img :src="Arrow" class="w-20 h-20" id="arrow" />
        <img :src="Body" class="w-20 h-20" />
      </div>
      <div class="mt-5">
        <Notification v-if="hits == hitsToWin" @new-game="newGame" message="¡Has Ganado!" :winner="true"  />
        <Notification v-else-if="missedAttempts == attemptsToLose" @new-game="newGame" message="Perdiste :(" :winner="false"   />
        <Question v-else :hits="hits" :hits-to-win="hitsToWin" @missed-attempt="missedAttempt" @increment-hit="incrementHit" />
      </div>
    </main> 
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import ArcheryLogo from './assets/archery-logo.svg'
import Arrow from './assets/arrow.svg'
import Body from './assets/body.svg'

import Question from './components/Question.vue'
import Notification from './components/Notification.vue';

const boxWidth = ref(0)
const traslateArrow = ref('')
const missedAttempts = ref(0)
const hits = ref(0)
const hitsToWin = 5
const attemptsToLose = 5

onMounted(() => {
  boxWidth.value = window.getComputedStyle(document.getElementsByTagName('main')[0]).width
})

function missedAttempt() {
  
  missedAttempts.value++
  traslateArrow.value = `${(parseInt(boxWidth.value.split('px')[0] - 80) / 5) * missedAttempts.value}px`
}

function incrementHit() {
  hits.value++
}

function newGame() {
  missedAttempts.value = 0
  hits.value = 0
  traslateArrow.value = '0px'
}

</script>

<style scoped>
#arrow {
  transform: translateX(v-bind('traslateArrow'));
  transition: ease .3s;
}

</style>
