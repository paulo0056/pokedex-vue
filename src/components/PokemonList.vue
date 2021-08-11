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
        <v-row justify="center .d-sm-none .d-md-flex">
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
                :src="imageUrl + (index + 1 + 20 * page) + '.svg'"
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
      <v-row justify="center mt-3">
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

    <!-- <v-row  align="center" justify="center">
      <v-col v-for="(pokemon, index) in pokemons.results" :key="index" cols="12" md="5" sm="5" lg="3" xl="3">
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
      
        

      </v-row> -->
    <v-dialog v-model="showDialog" width="500"
      ><div class="bg">
        <img
          :src="imageUrl + id + '.svg'"
          width="70"
          height="70"
          class="poke-img"
        />
        <v-text></v-text>
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
      pokemons: [],
      show: false,
      id: 0,
      page: 0,
      showDialog: false,
      detail: []
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
      console.log(this.contador);
      this.show = true;
      this.page = this.page + 1;
    },
    previous() {
      this.fetchPokemons(this.pokemons.previous);
      this.contador--;
      if (this.contador == 0) {
        this.show = false;
      }
      this.page = this.page - 1;
    },
    dialogOn(index) {
      this.showDialog = true;
      this.id = index + 1;
      fetch(
        `https://pokeapi.co/api/v2/pokemon/${index + 1 + 20 * this.page}`
      ).then((res) => {this.detail = res.json()} );
     console.log(this.detail)
      
    },
  },
  created() {
    /* this.fetchData();  */
    this.fetchPokemons();
  },
};
</script>

<style>
.search-box {
  border: solid 1px;
  height: 50px;
}

@media (max-width: 960px) {
  .botoes {
    display: none !important;
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