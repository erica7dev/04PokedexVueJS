<template>
  <div class="column is-half is-offset-one-quarter">
     <h2 class="is-size-1">Pokedex</h2>
     <input type="text" placeholder="Buscar pelo nome..." v-model="busca" class="input is-rounded">
     <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button><br><br>
     <div v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
       <!--Editando props-->
        <Pokemon 
          :name="pokemon.name"
          :url="pokemon.url"
          :num="index + 1"
        />
     </div>
  </div>
</template>

<script>
import Pokemon from './components/Pokemon-App.vue';
import axios from 'axios'; //req. p/ api 
export default {
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      busca: ""
    }
  },

  created: function(){//pegando dados da api sempre que for carregado
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
      console.log("Pegou a lista de resultads");
    }).catch(err =>{
      console.log(err);
    })
  },

  components: {
    Pokemon,
  },

  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == "" || this.busca == ' '){
        this.filteredPokemons = this.pokemons
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toUpperCase() == this.busca.toUpperCase());
      }
    }
  }
}

 /*pesquisa pelo input
   computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name.toUpperCase() == this.busca.toUpperCase());
      }
    }
  }
    */
</script>

<style>
#buscaBtn{
  margin-top: 5px;
}

#app{
  color: hsla(229, 48%, 49%, 0.959);
  text-align: center;
  margin-top: 60px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;  /*Suaviza a fonte no nível do pixel,*/
  -moz-osx-font-smoothing: grayscale; /*suaviza telas de não retina*/
}

</style>
