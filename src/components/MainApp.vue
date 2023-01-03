<template>
    <main class="container-fluid">
        <div class="row">
            <MostPopular v-if="(films.length == 0) && (tvs.length == 0)" :mostPopularMoviesList="mostPopularMoviesList" :mostPopularTvsList="mostPopularTvsList" />
            <FilmsCards :films="films" :movieGenreList="movieGenreList" />
            <TvsCards :tvs="tvs" :tvsGenreList="tvsGenreList" />
        </div>
    </main>
</template>

<script>
import FilmsCards from './FilmsCard.vue'
import TvsCards from './TvsCard.vue'
import MostPopular from './MostPopular.vue'
export default {
    components: {
        FilmsCards,
        TvsCards,
        MostPopular,
    },
    props: {
        films: Array,
        tvs: Array,
        movieGenreList : Array,
        tvsGenreList : Array,
        mostPopularMoviesList : Array,
        mostPopularTvsList : Array,
    },
    data: function () {
        return {};
    },
    methods: {
        //! this method find some flags with different name and transform it in the same name of the library
        mapLang(lang) {
            switch (lang) {
                case "en":
                    return "gb";
                case "ja":
                    return "jp";
                case "ko":
                    return "kr";
                case "el":
                    return "gr";
                case "er":
                    return "il";
                case "fa":
                    return "ir";
                case "zh":
                    return "cn";
                case "cs":
                    return "cz";
                case "nb":
                    return "no";
                case "hi":
                    return "in";
                default:
                    return lang;
            }
        },


    },
}
</script>
<style scoped lang="scss">
@import '../assets/style/variable.scss';

main {

    min-height: calc(100vh - 100px);
    background-color: $bgColorMain;

    ul {
        list-style-type: none;
        border: 1px solid black;
    }
}
</style>