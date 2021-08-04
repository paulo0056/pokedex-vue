<template>
   <v-container class="mt-5">
      <v-row  align="center" justify="center">
      <v-col v-for="(pokemon, index) in pokemons.results" :key="index" cols="2" class="poke-card">
          
          <img :src="imageUrl + ((index+1) + 20 * page) + '.svg'" width="96" height="96" class="poke-img">
          <h3 class="poke-title">{{ pokemon.name }}</h3>
      </v-col>
      <v-row justify="center" class="col-12 mt-10">
          <v-btn
            v-show="show"
            depressed
            color="error"
            @click="previous"
            class="mr-5"
            >
            BACK
            </v-btn>
          <v-btn
            depressed
            color="primary"
            @click="next"
            >
            NEXT
            </v-btn>
            
      </v-row>
      
        

      </v-row>
    </v-container>
</template>

<script>
import axios from 'axios' 
export default {
    props: [
        'apiUrl',
        'imageUrl'
    ],
    data() {
        return {
            pokemons: [],
            nextUrl: '',
            contador : 0,
            show: false,
            index: '',
            page: 0
            
            

        }
    },
    methods: {
      /*    fetchData(){
            let req = new Request(this.apiUrl);
            fetch(req)
                .then((resp) => {
                    if (resp.status === 200)
                    return resp.json();
                    
                })
                .then((data) =>{
                    this.nextUrl = data.next;
                    data.results.forEach(pokemon => {
                        pokemon.id = pokemon.url.split('/')
                        .filter(funcion(part) { return !!part }).pop();
                        this.pokemons.push(pokemon);
                    })
                })
                .catch((error) => {
                    console.log(error);
                })
        } , */
          fetchPokemons(url = this.apiUrl){
            
        axios.get(url).then( res => {this.pokemons = res.data;})
                .catch(e => console.log(e));
        }, 
        
        next(){
            this.fetchPokemons(this.pokemons.next)

            this.contador ++;
            console.log(this.contador)
            this.show = true
            this.page = this.page+1
           
        },
        previous(){
            this.fetchPokemons(this.pokemons.previous)
            this.contador --;
            if(this.contador == 0){
                this.show = false
            }
            this.page = this.page - 1
        },
        contadorImagem(){
            this.contadorImg ++;
        }
    },
    created() {
        /* this.fetchData();  */
        this.fetchPokemons(); 
    },
}
</script>

<style>
 .poke-img{
     margin-left: 25%;
     display: flex;
    
 }
 .poke-title{
     margin-left: 25%;
 }
 .poke-card{
     background-color: white;
     margin: 5px;
     border-radius: 5px;
     /* box-shadow: 5px 5px 5px black; */
 }
</style>