<template>
  <div class="pc-container">
    <div class="pokemon-card">
      <div class="card_front" :style="
        this.pokemon.colorCard[1] ? 
        'background: linear-gradient(150deg, '+this.colors[this.pokemon.colorCard[0]]+' 50%, '+this.colors[this.pokemon.colorCard[1]]+' 50%)' : 
        'background-color: '+this.colors[this.pokemon.colorCard[0]]+';'
      ">
        <img :src="pokemon.urlImg"/>
        <div class="circle"></div>
        <h5 class="poke-id">#{{this.pokemon.id}}</h5>
        <h5 class="poke-name">{{name | upper}}</h5>
        <h5>
          {{pokemon.types}}
        </h5>
      </div>
      <div class="card_back">
        <div class="poke-stats-name">HP: {{this.pokemon.stats[0]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[0]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[0]+'%'"
        >
        </div>
        <div class="poke-stats-name">Attack: {{this.pokemon.stats[1]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[1]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[1]+'%'"
        >
        </div>
        <div class="poke-stats-name">Defense: {{this.pokemon.stats[2]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[2]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[2]+'%'"
        >
        </div>
        <div class="poke-stats-name">Special-Attack: {{this.pokemon.stats[3]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[3]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[3]+'%'"
        >
        </div>
        <div class="poke-stats-name">Special-Defense: {{this.pokemon.stats[4]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[4]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[4]+'%'"
        >
        </div>
        <div class="poke-stats-name">Speed: {{this.pokemon.stats[5]}}</div>
        <div class="poke-stats-bar"
          :style="'background: linear-gradient(to right, '+this.colors[this.pokemon.colorCard[0]]+' '+this.pokemon.stats[5]+'%, '+this.colors[this.pokemon.colorCard[0]]+'71 '+this.pokemon.stats[5]+'%'"
        >
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  created: function(){  
    axios.get(this.url).then(response => {
      this.pokemon.id = response.data.id
      this.pokemon.urlImg = require('./../assets/generation/'+this.generation+'/'+this.pokemon.id+'.png');
      // this.pokemon.urlImg = 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/'+response.data.id+'.png';
      // this.pokemon.urlImg = 'https://pokeres.bastionbot.org/images/pokemon/'+response.data.id+'.png';
      this.pokemon.name = response.data.name
      var poketypes = ''
      var pokemoncolortype = []
      var pokeStats = []
      response.data.types.forEach(function(value, index){
        if(index == 0){
          poketypes = value.type.name[0].toUpperCase() + value.type.name.slice(1)
        }else{
          poketypes += ' | ' + value.type.name[0].toUpperCase() + value.type.name.slice(1)
        } 
        pokemoncolortype.push(value.type.name)
      });  

      response.data.stats.forEach(function(value){
        pokeStats.push(value.base_stat)
      });

      this.pokemon.stats = pokeStats;
      this.pokemon.colorCard = pokemoncolortype;
      this.pokemon.types = poketypes;
    });
  },
  data(){
    return {
      pokemon: {
        id: 0,
        urlImg: '',
        name: '',
        types: '',
        colorCard: [],
        stats: []
      },
      colors: {
        grass: "#d2f2c2",
        poison: "#f7cdf7",
        fire: "#ffd1b5",
        flying: "#eae3ff",
        water: "#c2f3ff",
        bug: "#e0e8a2",
        normal: "#e6e6c3",
        electric: "#fff1ba",
        ground: "#e0ccb1",
        fighting: "#fcada9",
        psychic: "#ffc9da",
        rock: "#f0e09c",
        fairy: "#ffdee5",
        steel: "#e6eaf0",
        ice: "#e8feff",
        ghost: "#dbbaff",
        dragon: "#c4bdff",
        dark: "#a9abb0",
      }
    }
  },
  props: {
    name: String,
    url: String,
    generation: Number
  },
  filters: {
    upper: function(value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .pc-container {
    margin: 20px;
  }
  .pokemon-card {
    position: relative;
    height: 300px;
    width: 185px;

    box-shadow: 0 3px 15px rgba(100, 100, 100, 0.5);
    border-radius: 20px;

    transition: transform 1.5s;
    transform-style: preserve-3d;
  }

  .pc-container:hover .pokemon-card {
    transform: rotateY(0.5turn);
    cursor: pointer;
  }

  .card_front {
    z-index: -2;
  }

  .card_back,
  .card_front {
    position: absolute;

    top: 0;
    left: 0;
    right: 0;
    bottom: 0;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    border-radius: 20px;
    border: 4px solid #fff;
    backface-visibility: hidden;
    /* transform-style: preserve-3d; */
  }

  .card_back {
    transform: rotateY(0.5turn);
    background-color: #464646;
    color: white;
    border: 4px solid #464646;
  }

  .poke-stats-name {
    font-weight: bold;
    font-size: 0.8rem;
  }

  .poke-stats-bar {
    width: 70%;
    height: 8px;
    margin-bottom: 15px;
    /* border: 1px solid black; */
  }
 
  .poke-id {
    margin-top: 10px;
    border-radius: 20px;
    padding: 3px 10px;
    font-size: 1.1rem;
    background: rgba(0, 0, 0, 0.2);
  }
  .poke-name {
    font-weight: bold;
    text-align: center;
  }
  img {
    width: 70%;
  }
  .circle {
    padding: 65px;
    position: absolute;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 50%;
    top: 6%;
    z-index: -1;
  }


</style>
