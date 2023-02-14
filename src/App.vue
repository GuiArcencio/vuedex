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
    <div @click="search" class="searchbutton"><p>SEARCH</p></div>

    <Display :pokemon_name="pokemon_name" />
</template>

<style>
.searchbox {
    margin-top: 30px;
    width: 300px;
    height: 30px;
    font-size: 24px;
    padding: 10px 10px;
    border-radius: 10px;
    border-width: 0;
    background-color: rgb(97, 97, 97);
}

.searchbox:focus {
    outline-style: solid;
    outline-color: white;
}

.searchbox::placeholder {
    color: white;
    opacity: 0.5;
}

.searchbutton {
    cursor: pointer;
    background-color: rgb(202, 55, 55);
    font-weight: 700;
    font-size: larger;
    letter-spacing: 1.2px;
    padding-left: 11.2px;
    padding-top: 10px;
    padding-bottom: 10px;
    padding-right: 10px;
    border-radius: 10px;
    transition: background-color 0.3s;
}
.searchbutton:hover {
    background-color: rgb(158, 51, 51);
}

</style>
