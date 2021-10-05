<template>
  <div id="app">
    <br />
    <div class="column is-6 is-offset-one-quarter">
      <!-- EXIBINDO ARRAY POKEMONS-->
      <h4 class="title">Pokedex</h4>
      <br />
      <!-- INPUT DE BUSCA-->
      <input
        type="text"
        v-model="buscarPokemon"
        class="input is-rounded"
        placeholder="Buscar pokemon pelo nome"
      />
      <button
        @click="buscar"
        class="button is-primary"
        id="botaoBuscarPokemonNome"
        style="margin-top: 10px"
      >
        Buscar
      </button>

      <div v-for="(poke, index) in pokemonsFiltrados" :key="poke.url">
        <!-- PASSANDO AS VARIÁVEIS PARA O POKEMON-->
        <br />
        <Pokemon
          :numeroPokemonPokedex="index + 1"
          :nomeDoPokemon="poke.name"
          :urlDoPokemon="poke.url"
        />
      </div>
    </div>
  </div>
</template>

<script>
//axios
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      //criando variável e passando o array do "resposta" pra ela
      pokemons: [],
      //variável q será utilizada para acessar o pokemon filtrado
      pokemonsFiltrados: [],
      buscarPokemon: "",
    };
  },
  components: {
    Pokemon,
  },
  //funcao que sera executada quando a pg carregar
  created: function () {
    //método de requisicao get do axios
    //pegando 151 pokemons
    //then : vai receber a resposta da requisicao

    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((resposta) => {
        /*
      acessando a resposta.data.arrayResults
      console.log(resposta.data.results)
      */

        //passando a resposta para o array
        this.pokemons = resposta.data.results;

        //esse será constantemente filtrado
        this.pokemonsFiltrados = resposta.data.results;
      });
  },
  methods: {
    //buscando por botão
    buscar: function () {
      //resetando a lista de pokemons
      this.pokemonsFiltrados = this.pokemons;

      if (this.buscarPokemon == "" || this.buscarPokemon == " ") {
        //se for vazio
        this.pokemonsFiltrados = this.pokemons;
      } else {
        //caso contrário
        //As informações não serão perdidas com um novo array
        this.pokemonsFiltrados = this.pokemons.filter(
         pokemon => pokemon.name == this.buscarPokemon
        )
      }
    },
  },
  computed: {
    /*
        resultadoBusca: function (){
      if (this.buscarPokemon == '' || this.buscarPokemon == ' '){
        //se a buscar for vazia, retornará tudo
        return this.pokemons;
      }else{
        //se nao, retorna o pokemon cujo nome seja igual ao digitado
        //dessa forma já faz uma busca sem apertar o botão
        return this.pokemons.filter(pokemons => pokemons.name == this.buscarPokemon);
      }
    }
    */
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
