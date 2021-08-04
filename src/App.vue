<template>
<v-app>
  <v-container>
    <div>
      <h2 class="titulo white--text">Pokedex</h2>
     <PokemonList :apiUrl="apiUrl" :imageUrl="imageUrl" />
    
    </div>
    
    
  </v-container>
</v-app>
  

</template>

<script>
/* import PokemonSearch from './components/PokemonSearch.vue'
import PokemonDetail from './components/PokemonDetail.vue' */
import PokemonList from './components/PokemonList.vue'
export default {
  components: { /* PokemonSearch, PokemonDetail, */ PokemonList},
  data() {
    return {
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/'
    }
  },
}
</script>

<style>
#app{
  background-color: #db5656;
}

   
  .titulo{
    justify-content: center;
    display: flex;
  }
  
</style>