<template>
    <div>
        <div class="container">
            <img class="logo" src="./assets/logo.png">
            <h1 class="text-center">
                {{title | uppercase }}
            </h1>
            <app-add-task
                    @emitAddTask="addTask($event)"
                    @emitRemoveAll="removeAll()"
                    :taskError="taskError">
            </app-add-task>
            <div class="row">
                <div class="col-md-offset-2 col-md-4">
                    <app-todo-tasks
                            :todoTasks="todoTasks"
                            @emitRemove="updateTodoTasks($event)"
                            @emitDone="done($event)">
                    </app-todo-tasks>
                </div>
                <div class="col-md-4">
                    <app-done-tasks
                            :doneTasks="doneTasks"
                            :todoTasks="todoTasks"
                            @emitRemove="updateDoneTasks($event)">
                    </app-done-tasks>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {TASKS} from "./data/moc-tasks";
    import AddTask from './components/AddTask';
    import TodoTasks from './components/TodoTasks';
    import DoneTasks from './components/DoneTasks';

    export default {
        components: {
            'app-add-task': AddTask,
            'app-todo-tasks': TodoTasks,
            'app-done-tasks': DoneTasks
        },
        data: function () {
            return {
                title: 'Tasks',
                todoTasks: TASKS,
                doneTasks: [],
                taskError: false,
                tasksLimit: 10
            }
        },
        methods: {
            updateTodoTasks(e) {
                return this.todoTasks = this.todoTasks.filter((item, index) => index !== e);
            },
            updateDoneTasks(e) {
                return this.doneTasks = this.doneTasks.filter((item, index) => index !== e);
            },
            done(obj) {
                if (!this.doneTasks.includes(obj.task)) {
                    this.doneTasks.unshift(obj.task);
                    this.updateTodoTasks(obj.index);
                }
            },
            addTask(task) {
                const tasks = this.todoTasks;
                if (task === '' || tasks.includes(task)) {
                    this.taskError = true;
                    return;
                }
                if (tasks.length < this.tasksLimit) {
                    this.taskError = false;
                    tasks.unshift(task);
                }
            },
            removeAll() {
                this.doneTasks = [];
                this.todoTasks = [];
                this.taskError = false;
            }
        },
        filters: {
            uppercase(val) {
                if (val) {
                    val = val.toString();
                    return val.toUpperCase();
                }
            }
        }
    }
</script>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css?family=Signika');

    $progress-bar__background--todo: #ff9f8d;
    $progress-bar__background--done: #41e252;

    body {
        background: #F5F5F5;
    }

    * {
        font-family: 'Signika', sans-serif
    }

    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 0.2s;
    }

    .fade-leave-to {

    }

    .fade-leave-active {
        transition: opacity 0.2s;
        opacity: 0;
    }

    .fly-enter {
        transform: translateX(-100%);
    }

    .fly-enter-active {
        transition: transform 0.4s;
    }

    .fly-leave-to {

    }

    .fly-leave-active {
        transform: translateX(100%);
        transition: transform 0.4s;
    }

    .fly-move {
        transition: transform 0.4s;
    }

    .logo {
        height: 100px;
        display: block;
        margin: 25px auto 0;
    }

    .c-progress {
        line-height: 40px;
        font-size: 20px;
        border: 1px solid black;
        position: relative;
        &__bar {
            top: 0;
            left: 0;
            position: absolute;
            height: 100%;
            transition: width 0.4s;
            z-index: -1;
            &--todo {
                background: $progress-bar__background--todo;
            }
            &--done {
                background: $progress-bar__background--done;
            }
        }
    }
</style>
