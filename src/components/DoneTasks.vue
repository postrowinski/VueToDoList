<template>
    <div>
        <h3>Done Tasks</h3>
        <transition name="fade" mode="out-in">
            <div v-if="doneTasks.length > 0" key="doneList">
                <div class="c-progress text-center">
                    {{doneTasks.length}} / {{doneTasks.length + todoTasks.length}}
                    <div class="c-progress__bar c-progress__bar--done"
                         :style="{width: progressWidth()}">
                    </div>
                </div>
                <ul class="list-group">
                    <transition-group name="fly">
                    <li v-for="(task, index) in doneTasks"
                        :key="task"
                        :style="{background: quantityColor()}"
                        class="list-group-item">
                        <span>{{task}}</span>
                        <div>
                            <button class="btn btn-danger"
                                    @click="remove(index)">
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
        name: "DoneTasks",
        props: ['doneTasks', 'todoTasks'],
        data() {
            return {
                remove(i) {
                    this.$emit('emitRemove', i);
                },
                progressWidth() {
                    const doneLength = this.doneTasks.length;
                    const todoLength = this.todoTasks.length;
                    return `${doneLength / (doneLength + todoLength) * 100}%`
                }
            }
        },
        methods: {
            quantityColor() {
                return `rgba(70, ${80 + this.doneTasks.length * 40}, 100, 0.5)`;
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
</style>