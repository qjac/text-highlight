<template>
    <div id="app">
        <Question />

        <!-- <Response /> -->
    </div>
</template>

<script>
import Question from './components/Question.vue';
// import Response from './components/Response.vue';

export default {
    name: 'App',
    components: {
        Question,
        // Response,
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
