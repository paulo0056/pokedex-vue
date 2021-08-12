<template>
  <v-container class="mt-2">
    <v-row justify="center">
      <v-col cols="12" sm="6" md="3">
        <form action="">
          <v-text-field
            class="search-box"
            placeholder="Pesquise seu pokemon"
            solo
            background-color="red"
          ></v-text-field>
        </form>
      </v-col>
    </v-row>

    <v-row align="center">
      <v-col cols="2">
        <v-row justify="center">
          <v-btn
            v-show="show"
            depressed
            color="error"
            @click="previous"
            class="botoes"
          >
            BACK
          </v-btn>
        </v-row>
      </v-col>
      <v-col>
        <v-row align="center" justify="center">
          <v-col
            v-for="(pokemon, index) in pokemons.results"
            :key="index"
            cols="12"
            md="4"
            sm="5"
            lg="3"
            xl="3"
          >
            <div class="poke-card" @click="dialogOn(index)">
              <img
                :src="imageUrl + ((index + 1) + 20 * page) + '.png'"
                width="70"
                height="70"
                class="poke-img"
              />
              <h3 class="poke-title">{{ pokemon.name }}</h3>
            </div>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="2">
        <v-row justify="center">
          <v-btn depressed color="primary" @click="next" class="botoes">
            NEXT
          </v-btn>
        </v-row>
      </v-col>
    </v-row>
    <div>
      <v-row justify="center">
        <v-btn
          v-show="show"
          depressed
          color="error"
          @click="previous"
          class="botoes-lg"
        >
          BACK
        </v-btn>
        <v-btn depressed color="primary" @click="next" class="ml-5 botoes-lg">
          NEXT
        </v-btn>
      </v-row>
    </div>

    <v-dialog v-model="showDialog" width="550"
      ><div class="bg">
        <img
          :src="imageUrl + id + '.png'"
          width="200"
          height="200"
          class="poke-dialog"
        />
        <v-row>
          <v-col cols="12" class="d-flex justify-center">
            <h2 class="dialog-title red--text">{{detail.name}}</h2>
          
        </v-col>
        <v-col cols="6" class="d-flex justify-center ml-9 nome-poke-sm">
          <h4 class="dialog-title brown--text">Peso:</h4>
          <h4 class="dialog-title"> {{detail.weight}}kg</h4>
         
        </v-col>
        <v-col cols="3" class="d-flex justify-center">
          <h4 class="dialog-title brown--text">Altura:</h4>
          <h4 class="dialog-title">{{detail.height}}m</h4>
         
        </v-col>
         <v-col cols="6" class="d-flex justify-end">
          <h4 class="dialog-title">Tipo:</h4>
         
        </v-col>
        
        <v-col cols="1"  v-for="(type, t) in detail.types"
           :key="t" class="d-flex justify-center">   
             <ul><li><h4 class="dialog-title pink--text">{{type}}</h4></li></ul>
        </v-col>
        </v-row>
        
        <div  >
          
        </div>
         <v-row justify="center" align="center" class="ma-2 mt-7">
           <v-col cols="2" v-for="(item, s) in status"
           :key="s">
             <div class="d-flex justify-center font-weight-bold green--text"><div class="h5-new">{{item.nome}}</div></div>
           </v-col>
           <v-col cols="2"  v-for="base_stat in detail.stats"
           :key="base_stat.effort">
             <div class="d-flex justify-center font-weight-bold indigo--text"><h4>{{base_stat}}</h4></div>
           </v-col>
           
           
          
        </v-row>
       
     
    
      
      

        </div
    ></v-dialog>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  props: ["apiUrl", "imageUrl"],
  data() {
    return {
      contador: 0,
      pokemons: [],
      show: false,
      id: 0,
      page: 0,
      showDialog: false,
      detail: [],
      tipos: {},
      status:[
        {nome: 'HP'}, 
        {nome: 'ATK'}, 
        {nome: 'DEF'}, 
        {nome: 'SP.ATK'},
        {nome: 'SP.DEF'},
        {nome: 'SPEED'},

      ]
     
    };
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
    fetchPokemons(url = this.apiUrl) {
      axios
        .get(url)
        .then((res) => {
          this.pokemons = res.data;
        })
        .catch((e) => console.log(e));
    },

    next() {
      this.fetchPokemons(this.pokemons.next);
      this.show = true;
      this.page = this.page + 1;
      this.contador ++;
    },
    previous() {
      this.fetchPokemons(this.pokemons.previous);
         this.page = this.page - 1;
         this.contador --;
      
      if(this.contador == 0){
         this.show = false
      } 
          
     
    },
    async dialogOn(index) {
      this.id = index + 1 + 20 * this.page;
     /*  await axios
        .get(`https://pokeapi.co/api/v2/pokemon/${index + 1 + 20 * this.page}`)
        .then((res) => {
          this.detail = res.data;
          this.tipos = res.data.map(type => {
          return {
          typeName: type.name
          }
          })
          
        })
        .catch((e) => console.log(e)); */
        const data = await fetch(`https://pokeapi.co/api/v2/pokemon/${index + 1 + 20 * this.page}`).then(res => res.json());
         this.detail = {
          name: data.name,
          number: data.id,
          weight: data.weight,
          height: data.height,
          types: data.types.map(({ type }) => type.name),
          stats: data.stats.map((stats) => stats.base_stat)
        }

        
         this.showDialog = true
      
            
   
     
     
    },
  },
  created() {
    /* this.fetchData();  */
    this.fetchPokemons();
  },
};
</script>

<style>

.atks{
  margin: 10px;
}
.dialog-title{
  margin-right: 40px;
  text-transform: capitalize;
}
.poke-dialog{
  margin-left: 25%;
  margin-top: 30px;
}
li{
  display: inline-block;
}
.h5-new{
  font-size: 15px;
}
ul{
  list-style-type: none;
  display: inline-block;
}
.search-box {
  border: solid 1px;
  height: 50px;
}

@media (max-width: 960px) {
  .botoes {
    display: none !important;
  }
  .dialog-title{
  margin-right: 0;
}
.h5-new{
  font-size: 13px;
}

}
@media (min-width: 960px) {
  .botoes-lg {
    display: none !important;
  }
}

@media (min-width: 1700px) {
  .next {
    margin-left: 20%;
  }
  .search-box {
    border: solid 1px;
    height: 50px;
    width: 80%;
  }
}

.bg {
  background-color: #fff;
  height: 550px;
}

.poke-card {
  padding: 10px;
  background-color: #fff;
  height: 130px;
  display: grid;
  text-align: center;
  justify-content: center;
  text-transform: capitalize;
  align-items: center;
  border-radius: 5px;
  cursor: pointer;

  /* box-shadow: 5px 5px 5px black; */
}
</style>