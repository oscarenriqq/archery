<template>
    <div class="text-3xl md:text-9xl font-extrabold" :class="[ props.winner ? 'text-yellow-400' : 'text-black' ]">
        {{ props.message }}
    </div>
    <p @click="$emit('newGame')" class="mt-5 underline cursor-pointer font-extrabold">Jugar de nuevo</p>
</template>

<script setup>
import { defineEmits, defineProps } from 'vue'

defineEmits(['newGame'])
const props = defineProps(['message', 'winner'])

</script>
