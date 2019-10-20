<template>
    <div>
        <img src="../assets/logo.png" alt="" class="logo">
        <input type="text" class="todo-input" placeholder="What needs to be done?"
               v-model="newTodo" @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
                <div class="todo-item-left">
                    <input type="checkbox" v-model="todo.completed">
                    <div v-if="!todo.editing" @dblclick="editTodo(todo)"
                         class="todo-item-label" :class="{completed : todo.completed}">
                        {{todo.title}}
                    </div>
                    <!--Add the v-focus directive-->
                    <input v-else type="text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"
                           @keyup.esc="cancelEdit(todo)" class="todo-item-edit" v-model="todo.title" v-focus>
                </div>
                <div class="remove-item" @click="removeTodo(index)">x</div>
            </div>
        </transition-group>
        <div class="extra-container">
            <div>
                <label>
                    <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"
                    >
                    Check All
                </label>
            </div>
            <div>{{ remaining }} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
                <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
                <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
            </div>
            <div>
                <transition name="fade">
                    <button v-if="showClearCompletedButton" @click="clearCompleted()">Clear completed</button>
                </transition>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'todo-list',
        data() {
            return {
                newTodo: '',
                beforeEditCache: '',
                idForTodo: 3,
                filter: 'all',
                todos: [
                    {
                        'id': 1,
                        'title': 'start a new vue project',
                        'completed': true,
                        'editing': false
                    }, {
                        'id': 2,
                        'title': 'create git repo',
                        'completed': false,
                        'editing': false

                    }
                ]
            }
        },
        computed: {
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            anyRemaining() {
                return this.remaining != 0
            },
            todosFiltered() {
                if (this.filter == 'active') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter == 'completed') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos // filter == all
            },
            showClearCompletedButton() {
                return this.todos.filter(todo => todo.completed).length > 0
            }
        },
        directives: {
            focus: {
                inserted: function (el) {
                    el.focus()
                }
            }
        },
        methods: {
            addTodo() {
                if (this.newTodo.trim() == '') {
                    return
                }
                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false
                })
                this.newTodo = '';
                this.idForTodo++;
            },
            removeTodo(index) {
                this.todos.splice(index, 1)
            },
            editTodo(todo) {
                this.beforeEditCache = todo.title;
                todo.editing = true;
            },
            doneEdit(todo) {
                if (todo.title.trim() == '') {
                    todo.title = this.beforeEditCache;
                }
                todo.editing = false;
            },
            cancelEdit(todo) {
                todo.title = this.beforeEditCache;
                todo.editing = false;
            },
            checkAllTodos() {
                this.todos.forEach((todo) => todo.completed = event.target.checked)
            },
            clearCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed)
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");
    .logo {
        display: block;
        margin: 10px auto;
        height: 100px;
    }

    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus {
            outline: 0;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
    }

    .remove-item {
        background: #000000;
        color: #fff;
        border: none;
        padding: 3px 7px;
        border-radius: 50%;
        cursor: pointer;

        &:hover {
            background: #ff0000;
        }
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 20px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;

        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        font-weight: bold;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgray;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;

        &:hover {
            background: lightgreen;
        }

        &:focus {
            outline: none;
        }
    }

    .active {
        background: lightgreen;
    }

    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>
