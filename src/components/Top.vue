<template>
    <div class="top">
        <div id="logo" @click="home">
            <img src="../assets/logo.png" height="40px"> 
            <div id="text">   
                Pokedex
            </div>
        </div>
        <div class="search">
            <div class="search-container">
                <input type="text" v-model="search" @keyup="sendBack" placeholder="Pikachu...">
                <select v-model="type_filter" @change="Filter" class="select-box">
                    <option value="" selected >All types</option>
                    <option v-for="option in options"  :key="option" v-bind:value="option.value">
                        {{ option.text | capitalize}}
                    </option>
                </select>
            </div>
        </div>
    </div>     
</template>

<script>
export default {
  name: 'Top',
  props: {
  },
  data(){
    return{
        search:"",
        options: [
            { value: 'bug', text: 'Bug' },
            { value: 'dark', text: 'dark' },
            { value: 'dragon', text: 'dragon' },
            { value: 'electric', text: 'electric' },
            { value: 'fairy', text: 'fairy' },
            { value: 'fighting', text: 'fighting' },
            { value: 'fire', text: 'fire' },
            { value: 'flying', text: 'flying' },
            { value: 'ghost', text: 'ghost' },
            { value: 'ground', text: 'ground' },
            { value: 'grass', text: 'grass' },
            { value: 'ice', text: 'ice' },
            { value: 'normal', text: 'normal' },
            { value: 'poison', text: 'poison' },
            { value: 'psychic', text: 'psychic' },
            { value: 'rock', text: 'rock' },
            { value: 'steel', text: 'steel' },
            { value: 'water', text: 'water' }
        ],
        type_filter: ""
    }
  },
  methods: {
    sendBack: function() {
        this.closeStats();
          window.scrollTo({
            top: 0,
            behavior: 'smooth',
          });
        this.$emit("page", 0);
        this.$emit("search", this.search);
    },
    home: function() {
        this.search = "";
        this.type_filter = "";
        this.closeStats();
          window.scrollTo({
            top: 0,
            behavior: 'smooth',
          });
        this.$emit("search", this.search);
        this.$emit("filter", this.type_filter);
        this.$emit("page", 0);
    },
    Filter: function() {
        this.closeStats()
          window.scrollTo({
            top: 0,
            behavior: 'smooth',
          });
        this.$emit("filter", this.type_filter);
    },
    closeStats: function(){
        document.getElementById('stats').style.transform="translateY(-100%)";
        setTimeout(function(){
            document.getElementById('stats').style.display="none";
        }, 200);
    }
  },
    filters: {
    capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }
    
}
</script>

<style>
    @import "../assets/top.css"; 
</style>