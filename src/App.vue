<template>
    <div id="app">
        <stream></stream>
        <div class="container-fluid">
            <h1 class="mb-3">Just Another Movie</h1>
            <search></search>
            <movies></movies>
        </div>
    </div>
</template>

<script>
import Stream from './components/Stream.vue';
import Search from './components/Search.vue';
import Movies from './components/Movies.vue';

export default {
    components: {
        Stream: Stream,
        Search: Search,
        Movies: Movies
    }
}
</script>

<style>

#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

</style>
