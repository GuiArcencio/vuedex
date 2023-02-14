<script setup lang="ts">
import { ref, watch } from 'vue'
import type { Ref } from 'vue'

import type { Pokemon } from '@/types';

enum DisplayStatus { Idle, Loading, Shown, NotFound, Error }

function capitalize(str: string): string {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase()
}

const props = defineProps({
    pokemon_name: {
        type: String,
        required: true
    }
})

const status = ref(DisplayStatus.Idle)
const pokemon_shown: Ref<Pokemon> = ref({
    name: '',
    sprite: '',
    type1: '',
})

// Fetch data from the API
watch(() => props.pokemon_name, async (poke) => {
    status.value = DisplayStatus.Loading

    const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${poke.toLowerCase()}`)
    switch (response.status) {
        case 200:
            const data = await response.json()

            pokemon_shown.value = {
                name: data.name,
                sprite: data.sprites.other.home.front_default,
                type1: data.types[0].type.name,
                type2: data.types[1]?.type.name
            }
            status.value = DisplayStatus.Shown
            break
        case 404:
            status.value = DisplayStatus.NotFound
            break
        default:
            status.value = DisplayStatus.Error
    }
})

</script>

<template>
    <div v-if='status == DisplayStatus.Idle'>
        <h3>Search for a Pokémon!</h3>
    </div>

    <div v-else-if='status == DisplayStatus.Loading'>
        <h3>...</h3>
    </div>

    <div v-else-if='status == DisplayStatus.Shown'>
        <img :src="pokemon_shown.sprite" width="300"/>
        <h3>{{ capitalize(pokemon_shown.name) }}</h3>
        <div class="typewrapper">
            <div class="type">{{ capitalize(pokemon_shown.type1) }}</div>
            <div v-if="pokemon_shown.type2 != null" class="type">{{ capitalize(pokemon_shown.type2) }}</div>
        </div>
    </div>

    <div v-else-if='status == DisplayStatus.NotFound'>
        <h3>I couldn't find any Pokémon called {{ pokemon_name }}</h3>
    </div>

    <div v-else-if='status == DisplayStatus.Error'>
        <h3>Something went wrong</h3>
    </div>
</template>
