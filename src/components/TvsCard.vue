<template>
    <div class="pt-4 container-fluid ps-4 mb-5 pb-5" v-if="(tvs != '')">
        <div class="row cont-card mx-auto">
            <div class="col-6">
                <h3>Lista Serie Tv:</h3>
            </div>
            <div class="col-6 pb-3">
                <span>Scegli un genere:</span>
                <select class="form-select w-50 d-inline-block ms-3" aria-label="Default select example" v-model="valueSelect">
                    <option selected value="">Tutti</option>
                    <option :value="element.id" v-for="(element , index) in tvsGenreList" :key="index">{{element.name}}</option>
                </select>
            </div>
            <hr class="ms-2">

            <!-- This answer appeare if a searched film has no genre required -->
            <div v-if="(searchByGenre(valueSelect) == '' )">Nessun film rispetta il tuo filtro</div>

            <!-- Card zone -->
            <div v-for="element in searchByGenre(valueSelect)" :key="element.id" class="img-card-size position-relative" @mouseleave="setFalseToIsActive()">
                <img :src="completePosterPathW342(element.poster_path)" :alt="element.title">
                <div class="position-absolute info pt-3 ps-3">
                    <img :src="completeBackPosterPathW342(element.backdrop_path)" alt="element.title" class="my-backposter">
                    <ul>
                        <li><span class="fw-bolder">Titolo Originale: </span>{{ element.original_name }}</li>
                        <li><span class="fw-bolder">Voto: </span>{{ changeValueVote(element.vote_average) }}</li>
                        <li><span class="fw-bolder">Panoramica: </span>{{ element.overview }}</li>
                    </ul>
                </div>

                <!-- Arrows -->
                <div class="position-absolute arrow-info" v-if="(isActive == false)"
                    @click="changeStatusActive() ; getCreditsTvs(element.id)">
                    <i class="fa-solid fa-angle-left"></i>
                </div>
                <div class="position-absolute arrow-info-active" v-else @click="changeStatusActive()">
                    <i class="fa-solid fa-angle-right"></i>
                </div>

                <!-- Credits zone and genre zone -->
                <div class="credits-container position-absolute pt-3 ps-3" v-if="isActive">
                    <ul  class="my-padding">
                        <li>Cast:
                            <ul>
                                <li v-for="(name, index) in creditsList" :key="index">{{ creditsList[index].name }}</li>

                            </ul>
                        </li>
                    </ul>
                    <ul class="my-padding">
                        <li >Generi:
                            <ul>
                                <li v-for="(id, index) in element.genre_ids" :key="index">{{ mapGenres(id) }}</li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    data: function () {
        return {
            apiLink: 'https://api.themoviedb.org/3/tv/',
            apiKey: '/credits?api_key=290ef3b32debba72662776b92b209938',
            isActive: false,
            creditsList: [],
            valueSelect: '',
        }
    },
    methods: {
        //! methods that create a path for posters/backposters of the cards
        completePosterPathW342(path) {
            if (path != null || path == "") {
                path = `https://image.tmdb.org/t/p/w342/${path}`;
            }
            else {
                path = "/defaultw-342.jpg";
            }
            return path;
        },
        completeBackPosterPathW342(path){
            if (path != null || path == "") {
                path = `https://image.tmdb.org/t/p/w342/${path}`;
            }
            else {
                path = "/defaultbackw-342.png";
            }
            return path;
        },


        //! method to transform vote from 10 to 5 and apply the correspondents stars  
        changeValueVote(vote) {
            let voteIn5 = Math.ceil((vote * 5) / 10);
            let stars = "";
            switch (voteIn5) {
                case 0:
                    stars = "0";
                    break;
                case 1:
                    stars = "⭐";
                    break;
                case 2:
                    stars = "⭐⭐";
                    break;
                case 3:
                    stars = "⭐⭐⭐";
                    break;
                case 4:
                    stars = "⭐⭐⭐⭐";
                    break;
                case 5:
                    stars = "⭐⭐⭐⭐⭐";
                    break;
                default:
                    console.warn("voteIn5 non e' riconosciuto ", { voteIn5 });
            }
            return stars;
        },

        //! with this method you can get the array of Credits calling an API
        getCreditsTvs(id){
            this.creditsList = []
            axios.get(this.apiLink + id + this.apiKey)
                .then((result)=> {
                    this.creditsList = result.data.cast
                    this.creditsList.splice(5)
                })
            .catch((error)=>{
                console.warn(error)
            })
        },
        changeStatusActive(){
            this.isActive = !this.isActive
        },
        setFalseToIsActive(){
            this.isActive = false
        },

        //! this method search in movieGenreList to find a name by id 
        mapGenres(id){
            return this.tvsGenreList.find(element => element.id == id).name
        },

        //! this method filter films by genre 
        searchByGenre(genre){
            if(genre==''){
                return this.tvs
            }
            return this.tvs.filter((film)=> film.genre_ids.includes(genre))
        },
    },
    props: {
        tvs: Array,
        tvsGenreList: Array,
    },
}
</script>

<style>
</style>