<template>
    <div v-if="movies && movies.length" class="row">
        <div class="col-sm-6 col-md-4 col-xl-3 mb-3" :key="'movie'" v-for="movie of movies">
            <div class="card">
                <div class="card-block pb-3">
                    <img v-if="movie.poster_path" :src="'https://image.tmdb.org/t/p/w185/' + movie.poster_path">
                    <img v-else width="185" src="/static/poster_placeholder.jpg">
                </div>
                <div class="card-block pt-0">
                    <h3 class="card-title">{{ movie.title }}</h3>
                    <h6 class="card-subtitle text-muted mb-2">({{ movie.release_date.split('-')[0] }})</h6>
                    <p class="card-text" v-html="movie.overview"></p>
                    <p>
                        <b>Rating</b>: {{ movie.vote_average }} / 10
                        <small>({{ movie.vote_count }} votes)</small>
                    </p>
                    <button class="btn btn-primary" @click="newStream(movie)">Stream</button>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="stylus" scoped>
</style>
<script>
import { dataBus } from '../main.js';
import axios from 'axios';

export default {
    data: () => {
        return {
            movies: [],
        }
    },
    methods: {
        newStream(movie) {
            let release_date = movie.release_date.split('-');
            let year = release_date[0];
            let stream = movie.title + '&y=' + year;

            dataBus.$emit('newStream', {
                stream: stream,
                title: movie.title,
                overview: movie.overview,
                year: year,
                poster_path: movie.poster_path,
                vote_average: movie.vote_average,
                vote_count: movie.vote_count
            });

            $('#stream').addClass('show').fadeIn();
            $('body').addClass('model-open')
                .append('<div class="modal-backdrop fade show"></div>');
        }
    },
    created() {
        dataBus.$on('newSearch', (search) => {
            this.movies = search;
        });
    }
}
</script>