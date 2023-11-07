<template>
    <h1 v-if="!pokemon">Espere por Favor</h1>
    <div v-else>
        <h1>¿Quien es ese pokemon? </h1>
  
    <pokemon-picture :pokemonId = "pokemon.id" :showPokemon = "showPokemon"/>
      <pokemon-options :pokemons = "pokemonArray" @selection = "checkAnswer" />
    </div>

    <template v-if="showAnswer">
        <h2 class="fade-in"> {{ message }}</h2>
        <button @click="newGame">Nuevo Guego</button>
    </template>
</template>

<script>

import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture' 
import getPokemonOptions from '@/helpers/getPokemonOption'


export default {
    components: { PokemonOptions, PokemonPicture, },
    data(){
        return {
            pokemonArray:[],
            pokemon: null,
            showPokemon: false,
            message: '',
            showAnswer: false,
        }
    },
    methods:{
        async mixPokemonArray(){
            this.pokemonArray = await getPokemonOptions()
            const rndInt = Math.floor(Math.random()*4)
            this.pokemon = this.pokemonArray[rndInt]
        },
        checkAnswer(selectedId){
            this.showPokemon= true
            this.showAnswer= true
            if(selectedId === this.pokemon.id){
                this.message =`Correcto,${this.pokemon.name}`
            }else{
                this.message =`Oops, era ${this.pokemon.name}`
            }
        },
        newGame(){
            this.showAnswer= false
            this.showPokemon= false
            this.pokemonArray = []
            this.pokemon = null
            this.mixPokemonArray()
        }
    },
    mounted(){
        this.mixPokemonArray()
    }

}
</script>