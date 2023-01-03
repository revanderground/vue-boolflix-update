<template>
    <div class="pt-4 container-fluid ps-4 mb-2 pb-5 ">
        <div class="row cont-card mx-auto">
            <h3>Film del momento:</h3>
            <hr class="ms-2">
            <div v-for="element in mostPopularMoviesList" :key="element.id" class="img-card-size position-relative">
                <img :src="completePosterPathW342(element.poster_path)" :alt="element.title">
                <div class="my-trailer-container position-absolute d-flex align-items-center justify-content-center flex-column">
                    <div class="txt-trailer">Guarda il trailer</div>
                    <div class="a-container">
                        <a :href="linkToYT(element)"><i class="fa-brands fa-youtube"></i>
                        </a>
                    </div>
                </div>
            </div>
        </div>
        
    </div>
</template>

<script>
export default {
    props :{
        mostPopularMoviesList : Array,
    },
    methods:{
        //! this method create a path for img and in case can't find the image set a new default img
        completePosterPathW342(path) {
            if (path != null || path == "") {
                path = `https://image.tmdb.org/t/p/w342/${path}`;
            }
            else {
                path = "/defaultw-342.jpg";
            }
            return path;
        },
        //! this method create a url replacing all the spaces with + and add the url of youtube
        linkToYT(element){
            let link = element.original_title.replaceAll(' ','+')
            return  'https://www.youtube.com/results?search_query='+link+'+trailer'
        },
        
    },
    data :  function(){
        return{
        
        }
    }
}
</script>

<style scoped lang="scss">
    
    .my-trailer-container{
        width: 100%;
        height: 100%;
        top: 0;
        left: 12px;
        background-color: rgba(0,0,0,0.7);
        visibility: hidden;
        opacity: 0;
        transition: visibility 0s ease-in 0ms, opacity 0ms;
        .txt-trailer{
            font-size: 2rem;
        }
        .a-container{
            a{
                font-style: none;
                color: rgb(255,0,0);
                font-size: 4rem;
                z-index:3;
            }
           
        }
        
    }
    .img-card-size:hover .my-trailer-container{
        visibility: visible;
        opacity: 1;
        transition: visibility 1s ease-in 0ms, opacity 300ms;
    }

</style>