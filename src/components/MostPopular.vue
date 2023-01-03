<template>
    <div>
        <MostPopularMoviesCards :mostPopularMoviesList="mostPopularMoviesList" />
        <MostPopularTvsCards :mostPopularTvsList="mostPopularTvsList" />
    </div>
</template>

<script>
import MostPopularMoviesCards from './MostPopularMoviesCards.vue'
import MostPopularTvsCards from './MostPopularTvsCards.vue'
export default {
    components :{
        MostPopularMoviesCards,
        MostPopularTvsCards,
    },
    props: {
        mostPopularMoviesList : Array,
        mostPopularTvsList : Array,
    }
}
</script>

<style>

</style>