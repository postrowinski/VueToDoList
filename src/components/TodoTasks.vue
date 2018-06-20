<template>
    <div>
        <h3>ToDo Tasks</h3>
        <transition name="fade" mode="out-in">
            <div v-if="todoTasks.length > 0" key="list">
                <div class="c-progress text-center">
                    {{todoTasks.length}} / {{limit}}
                    <div :style="{width: progressWidth()}" class="c-progress__bar c-progress__bar--todo"></div>
                </div>
                <ul class="list-group">
                    <transition-group name="fly">
                        <li v-for="(task, index) in todoTasks"
                            :key="task"
                            :class="addListItem()"
                            class="list-group-item">
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
                    </transition-group>
                </ul>
            </div>
            <div v-else key="empty">
                <p>Empty ToDo list</p>
            </div>
        </transition>
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
                this.$emit('emitRemove', index);
            },
            done(object) {
                this.$emit('emitDone', object);
            },
            addListItem() {
                return this.todoTasks.length > 4 ? 'list-group-item-danger' : 'list-group-item-success';
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