<template>
    <div class="brody-body">
        <h1>Is Adrien Brody in that Movie</h1>
        <input v-model="movieTitle" type="text" name="movie" placeholder="Movie Title" />
        <button v-on:click="fetchData">Go!</button>
        
        
        <div v-if="movie">
            <div v-if="broading"> 
                <span class="broading">BROADING</span>
            </div>  
            
            <div v-else>
                <img v-if="brodyResult.inMovie" src="./../assets/happy.jpg" alt="Happy Brody" />
                <img v-else src="./../assets/sad.jpg" alt="Sad Brody" />
                {{ brodyResult.msg }}
            </div>
        </div>

    </div>
</template>

<script>


    export default {
        name: "Search",
        data: function() {
            return {
                movieTitle: '',
                broading: false,
                movie: null,
                apiKey: process.env.VUE_APP_API_KEY,
            }
        },
        methods: {
            fetchData: async function() {
                this.broading = true;
                try {
                    const res = await fetch(`http://www.omdbapi.com/?apikey=${this.apiKey}&t=${this.movieTitle}`);
                    const movie = await res.json();

                    this.broading= false;
                    this.movie = movie;
                } catch(error) {
                    console.error(error);
                }
            }
        }, 

        computed: {
            brodyResult: function() {
                var movie = this.movie;
                
                if(movie.Actors.indexOf('Adrien Brody') > -1) {
                    return {
                        msg: 'YES! This is a Brody-ful movie!',
                        
                        inMovie: true
                    };
                } else {
                    return {
                        msg: 'NOOOOOOOOO!, why would they do this?',
                        inMovie: false
                    };
                }
            }
        }
    }
</script>

<style scoped>

    input {
        margin: 20px;
        font-size: 18px;
        padding: 5px;
    }

    button {
        font-size: 18px;
        text-transform: uppercase;
        display: block;
        margin:0 auto;
    }

    img {
        display: block;
        margin: 40px auto;
        max-width: 90%;
        height: auto;
        box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.5);
    }

    @keyframes pulse {

    }
</style>
