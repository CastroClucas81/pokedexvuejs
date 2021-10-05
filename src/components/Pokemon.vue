<template>
  <div>
    <div class="card">
      <div class="card-image">
        <figure>
          <!-- Estou dizendo que a imagem do pokemon é a atual -->
          <img :src="imagemAtual" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-6">
              <!-- | filter PrimeiraLetraNomePokemonMaiuscula-->
              {{ numeroPokemonPokedex }} -
              {{ nomeDoPokemon | PrimeiraLetraNomePokemonMaiuscula }}
            </p>
            <p class="subtitle is-6">
              <!-- Tipo do pokemon-->
              {{ pokemon.tipo }}
            </p>
            <small>{{ urlDoPokemon }}</small>
          </div>
        </div>

        <div class="content">
          <button class="button is-primary" @click="mudarSpritePokemon">Mudar sprite</button>
        </div>
      </div>
    </div>
    <br />
  </div>
</template>


<script>
import axios from "axios";

export default {
  //dados só são reativos dentro da função data
  data() {
    return {
      estaDeFrente: true,
      imagemAtual: "",
      pokemon: {
        tipo: "",
        frente: "",
        costas: "",
      },
    };
  },
  created: function () {
    //requisicoes no axios não são reativas

    //fazendo uma requisicao atraves da url de cada pokemon
    axios.get(this.urlDoPokemon).then((resposta) => {
      //criando no array pokemon uma variável chamada tipo
      //resposta.data.types é um caminho gerado na requisição
      //  resposta.data.types[0].type.name pegando um tipo do array tipes
      this.pokemon.tipo = resposta.data.types[0].type.name;

      //pegando a img de frente do pokemon
      this.pokemon.frente = resposta.data.sprites.front_default;

      //pegando a img de costas do pokemon
      this.pokemon.costas = resposta.data.sprites.back_default;

      //estou dizendo que a imagem inicial do pokemon é a de frente
      this.imagemAtual = this.pokemon.frente;
    });
  },
  props: {
    numeroPokemonPokedex: Number,
    nomeDoPokemon: String,
    urlDoPokemon: String,
  },
  filters: {
    PrimeiraLetraNomePokemonMaiuscula: function (oldName) {
      //slice remove a index de uma string
      //no caso, remoento a primeira letra minuscula do nome
      var newName = oldName[0].toUpperCase() + oldName.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSpritePokemon: function () {
      if (this.estaDeFrente) {
        this.estaDeFrente = false;
        this.imagemAtual = this.pokemon.costas;
      } else {
        this.estaDeFrente = true;
        this.imagemAtual = this.pokemon.frente;
      }
    },
  },
};
</script>

<style>
</style>