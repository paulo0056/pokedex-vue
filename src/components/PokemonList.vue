<template>
   <v-container class="mt-2">
      <v-row  align="center" justify="center">
      <v-col v-for="(pokemon, index) in pokemons.results" :key="index" cols="12" md="3" sm="5" lg="2" xl="2">
          <div class="poke-card">
              <img :src="imageUrl + ((index+1) + 20 * page) + '.svg'" width="70" height="70" class="poke-img">
          <h3 class="poke-title">{{ pokemon.name }}</h3>
          </div>
          
      </v-col>
      <v-row justify="center" class="col-12 mt-10">
          <v-btn
            v-show="show"
            depressed
            color="error"
            @click="previous"
            class="mr-5 botao"
            >
            BACK
            </v-btn>
          <v-btn
            depressed
            color="primary"
            @click="next"
            class="botao"
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
            page: 0,
            limit: 14,
            
            

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
     padding-top: 15px;
     background-color: white;
     height: 130px;
     margin: 5px;
     border-radius: 5px;
     cursor: pointer;
     
     /* box-shadow: 5px 5px 5px black; */
 }
 @media (min-width: 1700px)
{
  .poke-card
   {
    width: 170px !important;
   }
   .botao{
       margin-right: 100px !important;
   }
}
 @media (max-width: 600px)
{
  .poke-card
   {
   
   width: 170px;
   margin-left: 23%;
    
   }
} 


</style>