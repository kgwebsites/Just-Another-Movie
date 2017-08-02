<template>
    <div id="stream" class="modal fade">
        <div class="container" role="document">
            <div class="row">
                <div class="col-lg-8 offset-lg-2">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">{{ title }}
                                <small class="text-muted">({{ year }})</small>
                            </h5>
                            <button type="button" @click="streamClose" class="close" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <div class="embed-responsive embed-responsive-16by9" v-if="stream != ''">
                                <iframe :src="'//vodlocker.to/embed?t=' + stream + '&chromecast=0&trailer=0&fullscreen=1'" width="1280" height="720"></iframe>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <div class="row">
                                <div class="col-sm-4">
                                    <img class="mb-2" v-if="poster_path" :src="'https://image.tmdb.org/t/p/w185/' + poster_path">
                                    <img class="mb-2" v-else width="185" src="/static/poster_placeholder.jpg">
                                </div>
                                <div class="col-sm-8 align-self-center">
                                    <p v-html="overview"></p>
                                    <p>
                                        <b>Rating</b>: {{ vote_average }} / 10
                                        <small>({{ vote_count }} votes)</small>
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="stylus" scoped>
    img
        max-width 100%
    .modal
        overflow scroll
</style>

<script>
import axios from 'axios';
import { dataBus } from '../main.js';

export default {
    data: () => {
        return {
            stream: '',
            title: '',
            overview: '',
            year: '',
            poster_path: '',
            vote_average: '',
            vote_count: ''
        }
    },
    created() {
        dataBus.$on('newStream', (newStream) => {
            this.stream = newStream.stream;
            this.title = newStream.title;
            this.overview = newStream.overview;
            this.year = newStream.year;
            this.poster_path = newStream.poster_path;
            this.vote_average = newStream.vote_average;
            this.vote_count = newStream.vote_count;
        });
    },
    methods: {
        streamClose: () => {
            $('#stream').fadeOut();
            $('body').removeClass('model-open');
            $('.modal-backdrop').remove();
        }
    }
}
</script>