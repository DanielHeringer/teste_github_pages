<template>
    <div class="pokemons">
        <div v-if="n_pokemons < 1">
          No results for "{{search}}"
        </div>
        <div style="display:inline-block;" v-for="(name, index) in name_list" :key="name" >
            <div class="box" @click="openStats(String(id_list[index]))">
                
                <div class="imgbox" :id="'imgBox'+String(id_list[index])">
                  <img  v-if="id_list[index]>=100" :src="img_url+String(id_list[index])+img_type" @load="imgLoaded(id_list[index])" :id="String(id_list[index])" @error="imgerror(this)"  height="100%" width="100%">
                  <img  v-else-if="id_list[index]>=10" :src="img_url+'0'+String(id_list[index])+img_type" @load="imgLoaded(id_list[index])" :id="String(id_list[index])" @error="imgerror(this)"  height="100%" width="100%">
                  <img  v-else :src="img_url+'00'+String(id_list[index])+img_type" @load="imgLoaded(id_list[index])" :id="String(id_list[index])" @error="imgerror(this)"  height="100%" width="100%">
                </div>
                <div class="name" :style="'background-color:'+color_list[index]">
                    {{name | capitalize }}
                </div>
            </div>
        </div>
        <div class="pagination">
              <div v-for="p in (Math.ceil(n_pokemons/60))" :key="p" @click="sendPageBack(p-1)" style="display: inline-block;">
                <div id="selected" v-if="(p-1) === page_number" >
                  {{p}}
                </div> 
                <div class="item" v-else>
                  {{p}}
                </div>  
            </div>
        </div>
    </div>
</template>


<script>
export default {
  name: 'List',
  props: {
    name_list: Array,
    id_list: Array,
    type_list: Array,
    color_list: Array,
    search: String,
    page: Number,
    color_types: {},
    n_pokemons: Number
  },
  data(){
    return{
        img_url:"https://assets.pokemon.com/assets/cms2/img/pokedex/full/",
        img_type:".png",
        page_number: this.page,
        colorLoad: 1
    }
  },
  mounted: function () {
    
  },
  methods: {
        imgLoaded: function (id) {
          document.getElementById('imgBox'+id).style.background="transparent"
        },
        imgerror: function(img){
          img.src = "../assets/missing.png"
        },

        sendPageBack: function(p) {
          window.scrollTo({
            top: 0,
            behavior: 'smooth',
          });
            this.page_number = p;
            this.$emit("page", p);
        },
        openStats: function(id){
          this.$emit("statsID", id);
          document.getElementById('stats').style.display="block";
          setTimeout(function(){
            document.getElementById('stats').style.transform="translateY(0%)";
          }, 200);
          window.scrollTo({
            top: 0,
            behavior: 'smooth',
          });
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
    @import "../assets/list.css"; 
</style>
