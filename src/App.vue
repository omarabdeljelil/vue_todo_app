<template>
    <div id="app" class="container">
        <todo-list/>
    </div>
</template>

<script>
    import TodoList from './components/TodoList'

    export default {
        name: 'app',
        components: {
            TodoList
        }
    }
</script>

<style>

    * {
        box-sizing: border-box;
    }

    .container {
        max-width: 600px;
        margin: 0 auto;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
        font-size: 20px;
    }
</style>
