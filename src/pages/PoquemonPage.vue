<template>
  <h1 v-if="!pokemon">Espere por favor ...</h1>
  <div v-else>
    <h1>Quien es este pokemon ?</h1>
  <!-- img -->
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
  <!-- optiones -->
  <PokemonOptions :pokemons="pokemonArr"
  @selection="checkAnswer"
  />
  <template v-if="showAnswer">
    <h2 class="fade-in">{{ message }}</h2>
    <button @click="newGame">
      Nuevo juego
    </button>
  </template>
 
  </div>

  
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'
import getPokemonOptions from '@/helpers/getPokemonOptions'

//console.log(getPokemonOptions())
export default {
    components:{PokemonOptions,PokemonPicture},
    data(){
      return{
          pokemonArr:[],
          pokemon: null,
          showPokemon: false,
          showAnswer: false,
          message: ''

      }

    },
    methods:{
     async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptions()
        //console.log(this.pokemonArr)
        const rndInt =Math.floor(Math.random()*4)
        //console.log(rndInt)
        this.pokemon=this.pokemonArr[rndInt]
        console.log(this.pokemon)

      },
      checkAnswer(selectedId){
        this.showPokemon=true
        this.showAnswer=true
        if(selectedId===this.pokemon.id){
          this.message =`Correcto, ${this.pokemon.name}`
        }
        else{
          this.message=`Oops,era, ${this.pokemon.name}`

        }


        //console.log('pokemon page llamado',pokemonId)
      },
      newGame(){
        this.showPokemon = false
        this.showAnswer = false
        this.pokemonArr = []
        this.mixPokemonArray()
      }

    },
    // monta a la pagina
    mounted(){
      this.mixPokemonArray()

    }
}
</script>

