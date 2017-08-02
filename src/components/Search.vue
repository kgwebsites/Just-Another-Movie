<template>
    <form class="row mb-3" @submit.prevent="newSearch(search)">
        <div class="col-10">
            <input class="form-control" v-bind:value="search" v-model="search" placeholder="Search For Movie">
        </div>
        <div class="col-2">
            <button class="btn btn-success col-12">Search</button>
        </div>
        <div v-if="errors && errors.length">
            <p :key="error" v-for="error of errors">
                {{error.message}}
            </p>
        </div>
    </form>
</template>

<script>
import axios from 'axios';
import { dataBus } from '../main.js';

export default {
    data: () => {
        return {
            search: '',
            errors: ''
        }
    },
    methods: {
        newSearch(search) {
            axios.get(`https://api.themoviedb.org/3/search/movie?api_key=bd05e46f806ad4d0bb9f9a1b5176b7c1&language=en-US&query=` + search + `&include_adult=false`)
                .then(response => {
                    dataBus.$emit('newSearch', response.data.results);
                })
                .catch(e => {
                    this.errors.push(e)
                });
        }
    }
}
</script>