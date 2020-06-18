<template>
    <div id="app">
        <div class="container-fluid p-4">
            <img id="logo" class="mr-2" alt="Vue logo" src="./assets/logo.png">
            <h1 id="title" class="text-center">Tareas de {{name}}</h1>
            <div class="row bg-info p-4 m-2">
                <div class="col">
                    <input v-model="newTodoItem" class="form-control" placeholder="Escribe una tarea para empezar"
                           v-on:keyup.13="addNewTodo"/>
                </div>
                <div class="col-2">
                    <button class="btn btn-primary" v-on:click="addNewTodo">Añadir</button>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <label>
                        <input type="radio" name="taskDisplayFilter" v-on:click="displayTodo(0)"> Por hacer
                    </label>
                </div>
                <div class="col">
                    <label>
                        <input type="radio" name="taskDisplayFilter" v-on:click="displayTodo(1)"> Todos
                    </label>
                </div>
                <div class="col">
                    <label>
                        <input type="radio" name="taskDisplayFilter" v-on:click="displayTodo(2)"> Finalizados
                    </label>
                </div>
                <div class="col">
                    <label>
                        <input type="radio" name="taskDisplayFilter" v-on:click="displayTodo(3)"> Archivados
                    </label>
                </div>
            </div>

            <div class="row" v-if="filteredTasks.length === 0">
                <div class="col text-secondary text-center font-weight-bold p-2">
                    ¡Has acabado! Es hora de darse un respiro
                </div>
            </div>
            <template v-else>
                <div class="row">
                    <div class="col font-weight-bold m-3">Lista de tareas</div>
                </div>

                <div class="row p-3 border-bottom border-info" v-for="filteredTask in filteredTasks"
                     v-bind:key="filteredTask.id">
                    <div v-if="!filteredTask.editing" class="col">{{filteredTask.action}}</div>
                    <div v-else class="col"><input type="text" v-model="filteredTask.action"
                                                   v-on:keyup.13=toggleEditMode(filteredTask.id)></div>
                    <div class="col">
                        <template v-if="!filteredTask.done">
                            <i class="fa fa-square-o" aria-hidden="true"
                               v-on:click="toggleDoneStatus(filteredTask.id)"></i>
                        </template>
                        <template v-else>
                            <i class="fa fa-check-square-o" aria-hidden="true"
                               v-on:click="toggleDoneStatus(filteredTask.id)"></i>
                        </template>
                        <i class="fa fa-times ml-1" aria-hidden="true" v-on:click="deleteTask(filteredTask.id)"></i>
                        <template v-if="!filteredTask.isArchived">
                            <i class="fa fa-archive ml-1" aria-hidden="true"
                               v-on:click="toggleArchivedStatus(filteredTask.id)"></i>
                        </template>
                        <template v-else>
                            <i class="fa fa-file-excel-o ml-1" aria-hidden="true"
                               v-on:click="toggleArchivedStatus(filteredTask.id)"></i>
                        </template>
                        <i class="fa fa-pencil ml-1" aria-hidden="true"
                           v-on:click="toggleEditMode(filteredTask.id)"></i>

                    </div>
                </div>
            </template>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                "name": "Estefi",
                "tasks": [],
                "newTodoItem": "",
                "showing": 1
            };
        },
        computed: {
            filteredTasks() {
                switch (this.showing) {
                    case 0:
                        return this.tasks.filter( task => !(task.done || task.isArchived))
                    case 1 :
                        return this.tasks;
                    case 2 :
                        return this.tasks.filter(task => task.done);
                    case 3 :
                        return this.tasks.filter(task => task.isArchived);
                    default:
                        return this.tasks;
                }

            }
        },
        methods: {
            addNewTodo() {
                this.tasks.push({
                    "id": this.tasks.length,
                    "action": this.newTodoItem,
                    "done": false,
                    "editing": false,
                    "isArchived": false
                });
                localStorage.setItem("tasks", JSON.stringify(this.tasks))
                this.newTodoItem = ""
            },
            displayTodo(filterTag) {
                this.showing = filterTag
            },
            toggleDoneStatus(taskId) {
                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === taskId) {
                        this.tasks[i].done = !this.tasks[i].done;
                    }
                }
                localStorage.setItem("tasks", JSON.stringify(this.tasks));
            },
            toggleArchivedStatus(taskId) {
                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === taskId) {
                        this.tasks[i].isArchived = !this.tasks[i].isArchived;
                    }
                }
                localStorage.setItem("tasks", JSON.stringify(this.tasks));
            },
            toggleEditMode(taskId) {
                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === taskId) {
                        this.tasks[i].editing = !this.tasks[i].editing;
                    }
                }
                localStorage.setItem("tasks", JSON.stringify(this.tasks));

            },
            deleteTask(taskId) {
                this.tasks = this.tasks.filter(item => item.id !== taskId);
                localStorage.setItem("tasks", JSON.stringify(this.tasks));
            }
        },
        created() {
            const tasksStored = localStorage.getItem("tasks");
            if (tasksStored != null) {
                this.tasks = JSON.parse(tasksStored);
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

    #title {
        display: inline;
    }


</style>
