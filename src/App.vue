<template>
  <div id="app">
    <HeaderApp @filmSearched='setTxtSearched' />
    <MainApp :films="films" :tvs="tvs" :movieGenreList="movieGenreList" :tvsGenreList="tvsGenreList" :mostPopularMoviesList="mostPopularMoviesList" :mostPopularTvsList="mostPopularTvsList" />
  </div>
</template>

<script>
import HeaderApp from './components/HeaderApp.vue'
import MainApp from './components/MainApp.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    HeaderApp,
    MainApp,
  },
  data: function(){
    return{
      apiKey : '?api_key=290ef3b32debba72662776b92b209938',
      apiLink : 'https://api.themoviedb.org/3/search/',
      apiLinkGenreMovie : 'https://api.themoviedb.org/3/genre/movie/list',
      apiLinkGenreTvs : 'https://api.themoviedb.org/3/genre/tv/list',
      apiLinkPopularMovies : 'https://api.themoviedb.org/3/movie/popular',
      apiLinkPopularTvs : 'https://api.themoviedb.org/3/tv/popular',
      tv : 'tv',
      movie : 'movie',
      txtSearch : '',
      films : [],
      tvs : [],
      movieGenreList :[],
      tvsGenreList : [],
      mostPopularMoviesList : [],
      mostPopularTvsList :[],
      }
  },
  methods: {
    //! methods api call for movie, tv Series, Movie genre of movie/series and most popular
    APICallMovie() {
      axios.get(this.apiLink+this.movie+this.apiKey+this.txtSearch)
      .then((result) => {
        this.films = result.data.results
      })
      .catch((error) => {
        console.warn(error)
      })
    },
    APICallTvs(){
      axios.get(this.apiLink+this.tv+this.apiKey+this.txtSearch)
      .then((result) => {
        this.tvs = result.data.results
      })
      .catch((error) => {
        console.warn(error)
      })
    },
    APICallGenreMovie(){
      axios.get(this.apiLinkGenreMovie+this.apiKey)
      .then((result)=>{
        this.movieGenreList = result.data.genres
      })
      .catch((error)=>{
        console.warn(error``)
      })
    },
    APICallGenreTvs(){
      axios.get(this.apiLinkGenreTvs+this.apiKey)
      .then((result)=>{
        this.tvsGenreList = result.data.genres
      })
      .catch((error)=>{
        console.warn(error``)
      })
    },
    APICallMostPopularMovies(){
      axios.get(this.apiLinkPopularMovies+this.apiKey)
      .then((result)=>{
        this.mostPopularMoviesList = result.data.results
        this.mostPopularMoviesList.splice(4)
      })
      .catch((error)=>{
        console.warn(error)
      })
    },
    APICallMostPopularTvs(){
      axios.get(this.apiLinkPopularTvs+this.apiKey)
      .then((result)=>{
        this.mostPopularTvsList = result.data.results
        this.mostPopularTvsList.splice(4)
      })
      .catch((error)=>{
        console.warn(error)
      })    
    },
    //! Method that do the api call to pass to created
    setTxtSearched(choice){
      if(choice != ''){
        this.txtSearch = `&query=${choice}`
        this.APICallMovie()
        this.APICallTvs()
        this.APICallGenreMovie()
        this.APICallGenreTvs()
      }
      else{
        this.APICallMostPopularMovies()
        this.APICallMostPopularTvs() 
      }
    },
  },
  created (){
    this.setTxtSearched(this.txtSearch)
  }
}
</script>

<style lang="scss">
//! importing bootstrap, library for flags and fonts
@import "~bootstrap/scss/bootstrap";
@import "~flag-icons/css/flag-icons.css";
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300;0,400;0,500;0,700;0,800;1,300;1,400;1,500;1,600;1,700;1,800&display=swap');
body{
  color: white;
  font-family: 'Open Sans', sans-serif;

}
</style>
