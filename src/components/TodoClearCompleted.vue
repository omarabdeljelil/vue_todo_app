<template>
    <button v-if="showClearCompletedButton" @click="clearCompleted()">Clear completed</button>
</template>


<script>
    import {eventBus} from "../main";

    export default {
        name: 'todo-clear-completed',
        props: {
            showClearCompletedButton: {
                type: Boolean,
                required: true
            }
        },
        methods:{
            clearCompleted(){
                eventBus.$emit('clearCompletedTodos')
            }
        }
    }
</script>