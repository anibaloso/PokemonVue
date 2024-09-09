<script>
import axios from 'axios'
import ListadoPokemon from '@/components/ListadoPokemon.vue'
export default {
    components: {
        ListadoPokemon
    },
    data() {
        return {
            listadoPokemon: [],
            pokemonPreciso: {},
            aciertos: 0,
            escrito: ''
        }
    },
    methods: {
        //se llaman a todos los pokemon de la api
        async crearListado() {
            try {
                for (let i = 1; i <= 20; i++) {
                    const url = 'https://pokeapi.co/api/v2/pokemon/' + i
                    const { data } = await axios.get(url);
                    this.listadoPokemon.push(data);
                }
                this.listadoPokemon = this.listadoPokemon.map(pokemon => ({
                    ...pokemon,
                    esCorrecto: undefined
                }))
            } catch (error) {
                console.error('error es: ', error)
            }
        },
        //comparamos lo escrito con el pokemon correspondiente
        descubrir(pokemon) {
            if (pokemon.escrito.toLowerCase().trim() === pokemon.name.toLowerCase()) {
                this.aciertos = this.aciertos + 1
                pokemon.esCorrecto = true

            } else {
                console.log('no esta bien escrito')
            }
        }
    },
    computed: {
        //se calcula total de respuestas correctas a dado el usuario
        totalAciertos() {
            return this.listadoPokemon.filter((pokemon) => pokemon.esCorrecto).length
        }
    },
    mounted() {
        this.crearListado()
    }
}
</script>

<template>
    <div class="page">
        <div class="div">
            <img class="logo" src="../../public/png-clipart-pokemon-logo-pokemon-logo.png" alt="">
        </div>
        <h1>¿Quién es ese Pokémon?</h1>
        <p>Pokemones descubiertos: {{ totalAciertos }} </p>
        <div class="container">

            <!-- cargamos todo el listado de cards que se traen -->
            <ListadoPokemon :listadoPokemon="listadoPokemon" @pokemonDescubierto="descubrir" />

        </div>
    </div>
</template>

<style scoped>
.page {
    display: flex;
    flex-direction: column;
    text-align: center;

}

.logo {
    width: 20rem;
}

.container {
    display: flex;
    flex-direction: row;
}
</style>