<template>
  <div>
    <Top @search="updateSearch" @page="updatePage" @filter="updateFilter"/>
    <Stats :id="Number(stats_show)" :color_types="tipos_cor"/>
    <List :name_list="name_list" :color_list="color_list" :type_list="type_list" :n_pokemons="n_pokemons" :id_list="id_list" :search="search" :page="page" @page="updatePage" @statsID="updateStatsID" /> 
     
  </div>
</template>

<script>
import Top from './components/Top.vue'
import List from './components/List.vue'
import Stats from './components/Stats.vue'

export default {
  name: 'app',
  components: {
    Top,
    List,
    Stats
  },
  data(){
    return{
      pokemons: Object,
      page: 0,
      search: '',
      limit: 807,
      offset: (this.page*this.limit),
      id_list: [],
      name_list: [],
      type_list: [["normal"],["fighting"],["flying"],["poison"],
                    ["ground"],["rock"],["bug"],["ghost"],
                    ["steel"],["fire"],["water"],["grass"],
                    ["electric"],["psychic"],["ice"],["dragon"],
                    ["dark"],["fairy"]
                 ],
      color_list: [],
      typeSearch: "",
      n_pokemons: 0,
      stats_show: 1,
      tipos_cor: {bug: "#A8B820", dark: "#705848", dragon: "#7038F8",
                  electric: "#F8D030", fairy: "#EE99AC", fighting: "#C03028",
                  flying: "#A890F0",ghost: "#705898",ground: "#E0C068",
                  normal: "#705848",grass:"#78C850" , fire: "#E24242",
                  poison: "#A040A0", psychic: "#F85888",rock: "#B8A038",
                  steel: "#B8B8D0", ice:"#98D8D8", water:"#6890F0"}
    }
  },
  mounted: function(){
    fetch('https://pokeapi.co/api/v2/pokemon/?limit='+this.limit+'&offset='+this.offset, {
      method: 'get'
    })
    .then((response) =>{
      return response.json()
    })
    .then((response) =>{
      this.pokemons = response.results;
      this.getPokemonsTypes()
      this.generate_list(this.type_list);
    });
  },
  methods:{
    getIDFromURL: function(url){
        return (url.match(/(\d+)/g) || [])[1];
    },
    generate_list(typeList){
      this.name_list = []
      this.id_list = []
      this.color_list = []
      var i = 0
      this.n_pokemons = 0
      // pesquisa em branco
      if(this.search === ''){
        for(i=0; (i < Object.keys(this.pokemons).length); i++){
          
          // pesquisa em branco sem tipo
          if(this.typeSearch==="" && this.name_list.length < 60){
            if(this.n_pokemons < (60*(this.page+1)) && this.n_pokemons > (60*(this.page))){
              this.name_list.push(this.pokemons[i-1].name);
              this.id_list.push(this.getIDFromURL(this.pokemons[i-1].url));
            }
            this.n_pokemons++
          }
          // pesquisa em branco com tipo
          else{
            typeList.forEach(name => {
              if((name[0] === this.typeSearch && name.includes(this.pokemons[i].name)  && this.name_list.length <60) ){
                this.n_pokemons++
                if(this.n_pokemons < (60*(this.page+1)) && this.n_pokemons > (60*(this.page))){
                  this.name_list.push(this.pokemons[i].name);
                  this.id_list.push(this.getIDFromURL(this.pokemons[i].url)); 
                }
              }
            });
          }     
        }   
      }
      // pesquisa
      else{
        for(i=0; (i < Object.keys(this.pokemons).length); i++){
          // pesquisa sem tipo
          if(this.typeSearch==="" && this.name_list.length <60){
            if(this.pokemons[i].name.toUpperCase().includes(this.search.toUpperCase())){
              this.n_pokemons++
              if(this.n_pokemons < (60*(this.page+1)) && this.n_pokemons > (60*(this.page))){
                this.name_list.push(this.pokemons[i].name);
                this.id_list.push(this.getIDFromURL(this.pokemons[i].url));
              }
            }
          }
          // pesquisa com tipo
          else{
            if(this.pokemons[i].name.toUpperCase().includes(this.search.toUpperCase())){
              typeList.forEach(name => {
                if((name[0] === this.typeSearch && name.includes(this.pokemons[i].name)  && this.name_list.length <60) ){
                  this.n_pokemons++
                  if(this.n_pokemons < (60*(this.page+1)) && this.n_pokemons > (60*(this.page))){
                    this.name_list.push(this.pokemons[i].name);
                    this.id_list.push(this.getIDFromURL(this.pokemons[i].url)); 
                  }
                }
              });
            }
          }
        }
      }
    },
    getPokemonsTypes: function (){
        var url = "";
        for(var i=0; i<this.type_list.length;
              i++){
          url  = "https://pokeapi.co/api/v2/type/" + (i+1)
         fetch(url)
          .then((response) =>
            response.json())
          .then(response => {
            var names = []
            response.pokemon.forEach(element => {
              names.push(element.pokemon.name)
            })
            names.forEach(name => {
              this.type_list[response.id-1].push(name); 
            });
            })
        }
    },
    getByType(url){
      return fetch(url, {
        method: 'get'
      })
      .then((response) =>
         response.json())
    },
    updateSearch(s){
      this.search = s;
      this.getPokemonsTypes()
      this.generate_list(this.type_list)
    },
    updatePage(p){
      this.page = p;
      this.getPokemonsTypes()
      this.generate_list(this.type_list)
    },
    updateFilter(f){
      this.typeSearch = f
      this.page = 0
      this.getPokemonsTypes()
      this.generate_list(this.type_list)
    },
    updateStatsID(s){
      this.stats_show = s;
    }
  }
}
</script>

<style>
@import "font.css"; 

body{
    overflow-x: auto;
    background-color: #fff;
}
*{
    margin: 0;
    font: 20px 'Product Sans thin Regular';
    border: 0px solid black;
    transition: 0.3s;
}
a {
    color: inherit; /* blue colors for links too */
    text-decoration: inherit; 
    font: inherit;
}

::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
    color: #aaa;
    opacity: 1; /* Firefox */
}
</style>
