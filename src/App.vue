<template>
    <div id="app">
        <div class="container-fluid p-4">
            <img id="logo" class="mr-2" alt="Vue logo" src="./assets/logo.png">
            <h1 id="title" class="text-center">Tareas de {{name}}</h1>
            <div class="row bg-info p-4 m-2">
                <div class="col">
                    <input v-model="newTodoItem" class="form-control"/>
                </div>
                <div class="col-2">
                    <button class="btn btn-primary" v-on:click="addNewTodo">Añadir</button>
                </div>
            </div>

            <div class="row">
                <div class="col font-weight-bold m-3">Por hacer</div>
                <div class="col font-weight-bold m-3">Finalizado</div>
            </div>

            <div class="row" v-for="filteredTask in filteredTasks" v-bind:key="filteredTask.action">
                <div class="col">{{filteredTask.action}}</div>
                <div class="col">
                    <label>
                        <input type="checkbox" v-model="filteredTask.done" class="form-check-inline"/>
                    </label>
                </div>
            </div>


            <div class="row p-2">
                <div class="col text-center">
                    <input type="checkbox" v-model="hideCompleted" class="form-check-input">
                    <label class="form-check-label font-weight-bold">Ocultar finalizadas</label>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                "name": "Estefi",
                "tasks": [
                    {"action": "Ser la más bella", "done": true},
                    {"action": "Sacar fotos preciosas en la montaña", "done": false},
                    {"action": "Darse de alta en Malt.es", "done": false},
                    {"action": "Hacer el portfolio", "done": false}
                ],
                hideCompleted: true,
                newTodoItem: ""
            }
        },
        computed: {
            filteredTasks() {
                return this.hideCompleted ?
                    this.tasks.filter(task => !task.done) : this.tasks
            }
        },
        methods: {
            addNewTodo() {
                this.tasks.push({
                    "action": this.newTodoItem,
                    "done": false
                });
                this.newTodoItem = "";
            }
        }
    }
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

    #logo {
        padding-bottom: 12px;
        display: inline;
        width: 10%;
    }

    #title{
        display: inline;
    }


</style>
