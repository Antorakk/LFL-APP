<template>
    <ul class="responsive-table">
        <li id="header-table">
            <div class="col col-1">
                Pseudo
            </div>
            <div class="col col-2">
                Nom
            </div>
            <div class="col col-3">
                Prénom
            </div>
            <div class="col col-4">

            </div>
        </li>
        <li v-on:click="displayPlayer(player)" class="table-row" v-for="player,index in playersFilter" :key="index">
            <div class="col col-1" data-label="pseudo">
                <p>
                    {{player.pseudo}}
                </p>
            </div>
            <div class="col col-2" data-label="nom">
                <p>
                    {{player.nom}}
                </p>
            </div>
            <div class="col col-3" data-label="prenom">
                <p>
                    {{player.prenom}}
                </p>
            </div>
            <!-- <button class="col col-5 showPlayer" v-on:click="displayPlayer(player)">Afficher</button> -->
        </li>

    </ul>
</template>

<script>
import _ from 'lodash';
import axios from 'axios';
export default {
    name:"Vue-joueurs",
    props:[
        "nationalites",
        "searchPlayer"],
    data:function(){
        return {
            data_joueurs : null,
        }
    },
    computed:{
        playersFilter(){
            if (this.searchPlayer.length > 0)
                return _.filter(this.data_joueurs,
                    player => player.nom.includes(this.searchPlayer.toUpperCase()))
            else return this.data_joueurs;
        }
    },
    mounted(){
        axios
            .get("http://localhost:3000/players")
            .then((res) => {
                this.data_joueurs = res.data;
            })
            .catch((error) => {
                console.log("vueJoueurErreurMounted")
                console.log(error);
            })
    },
    methods:{
        displayPlayer(joueur){
            this.$emit('displayPlayer',joueur)
        }
    }
}
</script>

<style lang="scss">
.responsive-table{

        padding-inline-start: 0px;
        // margin: auto;
        width: calc(90% + 50px);
        li{
            border-radius: 10px;
            padding: 25px 30px;
            display: flex;
            justify-content: space-between;
            margin-bottom: 25px;
        }
        .table-row{
            background-color: #F8F8F8;
            box-shadow: 0px 1px 11px 0px rgba(0,0,0,0.4);
            transition: transform .4s;
        }
        .table-row:hover{
            transform: scale(1.05);
        }

        #header-table{
            margin-top: 50px;
            font-weight: bold;
            background-color: rgb(17, 17, 17);
            box-shadow: 0px 0px 9px 0px rgba(0,0,0,0.1);

            div{
                color: white;
                font-size: 20px;
            }
        }
        h1{
            color: black;
            margin-top: 75px;
            font-size: 25px;
        }
        .col-1{
            flex-basis:33%;
        }
        .col-2 {
            flex-basis: 33%;
        }
        .col-3 {
            flex-basis: 33%;
        }

        // .col-4{
        //     flex-basis: 8%;
        // }

    }
</style>