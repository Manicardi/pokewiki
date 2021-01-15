<template>
  <div id="app">
    <img class="banner" src="./assets/pokemon.webp">
    <h1 id="heading">Pokémon {{ generationRange[generation-1].name }}</h1>
    
    <Generation :generation="generation" :changeGeneration="changeGeneration" />
    <!-- <div class="search-box-container">
      <input type="text" class="search-box" placeholder="Search">
      <i class="fas fa-search"></i>
    </div> -->
    <div class="pokemon-container">
      <Pokemon v-for="pokemon in pokemons" :key="pokemon.name" :name="pokemon.name" :url="pokemon.url" :generation="generation"/>
    </div>
  </div>
</template>

<script>
import Pokemon from './components/Pokemon.vue'
import Generation from './components/GenerationMenu.vue'
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      generation: 1,
      pokemons: [],
      search: '',
      generationRange: [
        {
          name: 'Kanto',
          limit: 151,
          offset: 0
        },
        {
          name: 'Johto',
          limit: 100,
          offset: 151
        },
        {
          name: 'Hoenn',
          limit: 135,
          offset: 251
        },
        {
          name: 'Sinnoh',
          limit: 107,
          offset: 386
        },
        {
          name: 'Unova',
          limit: 156,
          offset: 493
        },
        {
          name: 'Kalos',
          limit: 72,
          offset: 649
        }
      ]
    }
  },
  created: function() {
    this.changeGenerationPokemon()
  },
  components: {
    Pokemon, 
    Generation
  },
  methods: {
    changeGeneration(generation) {
      if(generation == 'right'){
        if(this.generation > 1){
          this.generation--
          this.changeGenerationPokemon()
        }
      }else if(generation == 'left'){
        if(this.generation < this.generationRange.length){
          this.generation++
          this.changeGenerationPokemon()
        }
      }else{
        this.generation = generation
        this.changeGenerationPokemon()
      }
    },
    changeGenerationPokemon(){
      this.initDataLoaded = false;
      this.pokemons = []
      axios.get('https://pokeapi.co/api/v2/pokemon?limit='+this.generationRange[this.generation-1].limit+'&offset='+this.generationRange[this.generation-1].offset).then(response => {
        this.pokemons = response.data.results;
      })
    },
  },
}
</script>

<style>
  
.banner {
  width: 100%;
  max-width: 900px;
  margin: auto;
  margin-top: 30px;
}

/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */

h1 {
  font-size: 2.5rem;
}

h5 {
  font-size: 1.25rem;
}

h1, h5 {
  line-height: 1.2;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Segoe UI";
  color: #212529;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: stretch;

  overflow-y: scroll; /* To always have the scrollbar */

  background: #abbaab;
  background: linear-gradient(to right, #d0ece7, #edeed8);
}

h1 {
  align-self: center;
  margin: 30px 0 15px 0;
  font-family: "Lemonada", cursive;
  text-align: center;

  color: rgb(0, 0, 0, 0.8);
}

.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  /*Both these lines are only useful for the loader*/
  height: 100%;
  align-items: center;
}

/* +++++ */

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

#app {
  position: relative;
}

.fa-search {
  border-bottom: 3px solid rgb(0, 0, 0, 0.9);
  color: rgb(0, 0, 0, 0.9);
}

.search-box:focus {
  border-bottom: 3px solid darkred;
}

.search-box:focus + .fa-search {
  border-bottom: 3px solid darkred;
  color: darkred;
}

.button-container {
  align-self: center;
  margin-bottom: 15px;

  display: flex;
  align-items: center;
}

.button-container button {
  background: transparent;
  color: #464646;
  border: none;
  outline: none;
  font-size: 1.3rem;
  height: 30px;
  width: 30px;
  border: 3px solid #464646;
  border-radius: 50%;
  margin: 0 5px;
}

.button-container button i {
  pointer-events: none;
}

/* .button-container button:hover {
  cursor: pointer;
} */

.button-container .pagination {
  height: 30px;
  width: 30px;
  line-height: 30px;
  text-align: center;
  margin: 0 2px;
  font-weight: bold;
  transition: all 0.2s;
  user-select: none;
}

.current-page {
  color: whitesmoke;
  background: #464646;
  border-radius: 3px;
}

.button-container .restrict-click {
  color: lightgrey;
  border: 3px solid lightgray;
}

.search-box-container {
  align-self: center;
  width: 50%;
  display: flex;
  margin-bottom: 30px;
}

.search-box {
  width: 100%;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1.1rem;
  font-weight: bold;
  border-bottom: 3px solid rgb(0, 0, 0, 0.9);
  padding-bottom: 5px;
}

.loader {
  border: 10px solid rgb(0, 0, 0, 0.4);
  border-radius: 50%;
  border-top: 10px solid transparent;
  height: 65px;
  width: 65px;

  animation: spin 1.5s linear infinite;
}

</style>
