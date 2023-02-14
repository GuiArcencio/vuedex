<script setup lang="ts">
import { ref } from 'vue'

import Display from '@/components/Display.vue'

const search_text = ref('')
const pokemon_name = ref('')

function search() {
    pokemon_name.value = search_text.value
}

</script>

<template>
    <input v-model="search_text" placeholder="Pokémon name" class="searchbox"/>
    <div @click="search">SEARCH</div>

    <Display :pokemon_name="pokemon_name" />
</template>

<style>
.searchbox {
    width: 300px;
    height: 30px;
    font-size: 24px;
    padding: 10px 10px;
    color: #3a3a3a;
    border-radius: 10px;
}
</style>
