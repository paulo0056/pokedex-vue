<template>
<v-app>
  <v-container>
    <div>
      <v-row justify="center mt-1"> <div class="titulo"><v-img src="./assets/pokemon-logo.png" height="130" width="350" alt="pokemon"></v-img></div> </v-row>
     
     <PokemonList :apiUrl="apiUrl" :imageUrl="imageUrl" />
     
    
    </div>
    
    
  </v-container>
</v-app>
  

</template>

<script>
/* import PokemonSearch from './components/PokemonSearch.vue'*/

import PokemonList from './components/PokemonList.vue'


import Vue from 'vue'
export default Vue.extend(
  {
  components: { /* PokemonSearch,*/PokemonList},
  
  data() {
    return {
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/',
    
     

      
    }
  },
 

}
) 
</script>

<style>
  #app{
    /* background-color: #000000;
    background-image: linear-gradient(315deg, #000000 0%, #b82e1f 74%); */
    background-color: #990000;
    background-image: linear-gradient(147deg, #990000 0%, #ff0000 74%);
  }


  .titulo{
    justify-content: center;
    display: flex;
  }
   @media (min-width: 1700px)
{
  .titulo{
    margin-right: 100px;
  }
}

</style>