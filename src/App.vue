<template>
    <div>
        <div class="container">
            <h1 class="text-center">
                {{title | uppercase }}
            </h1>
            <app-add-task
                    @addTask="addTask($event)"
                    @removeAll="removeAll()"
                    :taskError="taskError">
            </app-add-task>
            <div class="row">
                <div class="col-md-offset-2 col-md-4">
                    <app-todo-tasks
                            :todoTasks="todoTasks"
                            @remove="updateTodoTasks($event)"
                            @done="done($event)">
                    </app-todo-tasks>
                </div>
                <div class="col-md-4">
                    <app-done-tasks
                            :doneTasks="doneTasks"
                            :todoTasks="todoTasks"
                            @remove="updateDoneTasks($event)">
                    </app-done-tasks>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
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
                todoTasks: ['Go to work', 'Create apps', 'Sleep'],
                doneTasks: ['dsda', 'dsada'],
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
                this.updateTodoTasks(obj.index);
                this.doneTasks.unshift(obj.task);
            },
            addTask(task) {
                const tasks = this.todoTasks;
                if (task === '' || tasks.includes(task)) {
                    this.taskError = true;
                    return null;
                }
                if (tasks.length < this.tasksLimit) {
                    this.taskError = false;
                    tasks.unshift(task);
                }
            },
            removeAll() {
                this.doneTasks = [];
                this.todoTasks = [];
            }
        },
        filters: {
            uppercase(val)
            {
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
        font-family: 'Signika', sans-serif;
        background: #F5F5F5;
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
