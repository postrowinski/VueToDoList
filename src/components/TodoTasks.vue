<template>
    <div>
        <h3>ToDo Tasks</h3>
        <div v-if="todoTasks.length > 0">
            <div class="c-progress text-center">
                {{todoTasks.length}} / {{limit}}
                <div :style="{width: progressWidth()}" class="c-progress__bar c-progress__bar--todo"></div>
            </div>
            <ul class="list-group">
                <li v-for="(task, index) in todoTasks"
                    class="list-group-item"
                    :class="addListItem()">
                    <span>{{task}}</span>
                    <div>
                        <button class="btn btn-success" @click="done({task, index})">
                            <i class="fa fa-check"></i>
                        </button>
                        <button class="btn btn-danger" @click="remove(index)">
                            <i class="fa fa-remove"></i>
                        </button>
                    </div>
                </li>
            </ul>
        </div>
        <div v-else>
            <p>No tasks todo</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TodoTasks",
        props: ['todoTasks'],
        data() {
            return {
                limit: 10
            }
        },
        methods: {
            progressWidth() {
                return `${this.todoTasks.length / this.limit * 100}%`;
            },
            remove(index) {
                this.$emit('remove', index);
            },
            done(object) {
                this.$emit('done', object);
            },
            addListItem() {
                return this.todoTasks.length > 1 ? 'list-group-item-danger' : 'list-group-item-success';
            }
        }
    }
</script>

<style lang="scss" scoped>
    ul {
        overflow: hidden;
    }

    li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin: 15px 0;
    }

    .btn {
        width: 40px;
    }

    .btn-success {
        margin-right: 6px;
    }
</style>