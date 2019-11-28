<template>
<div id="stats">
    <div class="box">
        <div class="type">
            <p v-for="types in pokemon.types" :key="types" :style="'background-color:'+color_types[types.type.name]">
                {{types.type.name | capitalize}}
            </p>
        </div>
        <div class="nameImgContainer">
            <img id="main_img" :src="img_url+id+img_type">
            <div class="name">
                #{{pokemon.id}} {{pokemon.name | capitalize}}
            </div>
        </div>
        <div class="statsContainer">
            <div class="statsPosition">
                <div class="statsBarBox">
                    HP:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="hp">
                            <p>{{pokemon.stats[5].base_stat}}</p>
                        </div>
                    </div>
                    Attack:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="attack">
                            <p>{{pokemon.stats[4].base_stat}}</p>
                        </div>
                    </div>
                    Defense:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="defense">
                            <p>{{pokemon.stats[3].base_stat}}</p>
                        </div>
                    </div>
                </div>
                <div class="statsBarBox">
                    Speed:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="speed">
                            <p>{{pokemon.stats[0].base_stat}}</p>
                        </div>
                    </div>
                    Special Attack:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="spatk">
                            <p>{{pokemon.stats[2].base_stat}}</p>
                        </div>
                    </div>
                    Special Def:
                    <div class="statsBar" >
                        <div class="statsBarIn" id="spdef">
                            <p>{{pokemon.stats[1].base_stat}}</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="info-container">
                <div class="info">
                    Abilities: <p v-for="abilitie in pokemon.abilities" :key="abilitie" >
                    {{abilitie.ability.name.replace("-", " ") | capitalize}}
                </p>
                </div>
                <!-- <div class="info">
                    Evolves to: 
                    <img id="e1" src="../assets/missing.png" v-if="n_evol > 0"> 
                    <img id="e2" src="../assets/missing.png" v-if="n_evol > 1">
                </div> -->
            </div>
        </div>
        <div class="moves">
            Moves:
                <b v-for="moves in pokemon.moves" :key="moves" >
                {{moves.move.name.replace("-", " ") | capitalize }},
                </b>  
        </div>
        <div class="back" @click="closeStats">
            <img src="../assets/arrow_up.png" width="50px">
        </div>
    </div>
</div>
    
</template>

<script>
export default {
    name: 'Stats',
    props: {
        id: Number,
        color_types: {}
    },
    data(){
        return{
            img_url:"https://pokeres.bastionbot.org/images/pokemon/",
            img_type:".png",
            pokemon: Object,
            n_evol: 3

        }
    },
    mounted: function () {
        this.updateStats();
    },
    watch: {
        id: function(){
            this.updateStats();
        }
    },
    methods: {
        updateStats: function () {
            var url = 'https://pokeapi.co/api/v2/pokemon/'+ this.id;
            fetch(url,{
                method: 'get'
            })
            .then((response) =>{
                return response.json();
            })
            .then((response) =>{
                this.pokemon = response;
                var speed = this.pokemon.stats[0].base_stat;
                var spdef = this.pokemon.stats[1].base_stat;
                var spatk = this.pokemon.stats[2].base_stat;
                var atk = this.pokemon.stats[4].base_stat;
                var def = this.pokemon.stats[3].base_stat;
                var hp = this.pokemon.stats[5].base_stat;
                if(document.getElementById('speed')){
                    setTimeout(function(){
                        document.getElementById('speed').style.width=String(speed)+"%";
                        document.getElementById('spdef').style.width=String(spdef)+"%";
                        document.getElementById('spatk').style.width=String(spatk)+"%";
                        document.getElementById('attack').style.width=String(atk)+"%";
                        document.getElementById('defense').style.width=String(def)+"%";
                        document.getElementById('hp').style.width=String(hp)+"%";
                    }, 400);
                }
            });
            url = "https://pokeapi.co/api/v2/evolution-chain/" + this.id
            console.log(url)
            fetch(url,{
                method: 'get'
            })
            .then((response) =>{
                return response.json();
            })
            .then((response) =>{
                if(response.chain){
                    document.getElementById('e1').src="https://assets.pokemon.com/assets/cms2/img/pokedex/full/"+(this.id+1)
                }
            })
        },
        closeStats: function(){
            document.getElementById('stats').style.transform="translateY(-100%)";
            setTimeout(function(){
                document.getElementById('stats').style.display="none";
            }, 200);
        },
        imgerror: function(id){
            document.getElementById('main_img').src = "https://assets.pokemon.com/assets/cms2/img/pokedex/full/"+id+this.img_type;
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
    @import "../assets/stats.css"; 
</style>