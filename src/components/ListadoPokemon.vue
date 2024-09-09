<script>
export default {
    props: {
        listadoPokemon: {
            type: Array,
            required: true
        }
    },
    methods: {
        descubrir(pokemon) {
            this.$emit('pokemonDescubierto', pokemon)
        }
    }
}
</script>

<template>

    <!-- se crea la card para cada pokemon -->
    <div class="row">
        <div class="col-3" v-for="(pokemon, idx) in listadoPokemon" :key="idx">
            <div v-if="pokemon.esCorrecto === undefined" class="card">
                <img :src="pokemon.sprites.front_default" alt="" :style="{ filter: 'blur(5px) grayscale(100%)' }">
                <input type="text" v-model="pokemon.escrito" @keypress.enter="descubrir(pokemon)">
                <button @click="descubrir(pokemon)">Descubrir</button>
            </div>
            <div v-else class="card">
                <img :src="pokemon.sprites.front_default" alt="">
                <p>{{ pokemon.escrito }}</p>

            </div>
        </div>
    </div>

</template>

<style scoped>
button {
    margin-top: 1rem;

}

.row {
    display: flex;
    flex-direction: row;

}

img {
    object-fit: contain;
}

.card {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
</style>
