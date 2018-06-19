<template>
    <div>
        <h3>Done Tasks</h3>
        <div v-if="doneTasks.length > 0">
        <div class="c-progress text-center">
            {{doneTasks.length}} / {{doneTasks.length + todoTasks.length}}
            <div class="c-progress__bar c-progress__bar--done"
                :style="{width: progressWidth()}">
            </div>
        </div>
        <ul class="list-group">
            <li v-for="(task, index) of doneTasks"
                class="list-group-item">
                <span>{{task}}</span>
                <div>
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
        name: "DoneTasks",
        props: ['doneTasks', 'todoTasks'],
        data() {
            return {
                remove(i) {
                    this.$emit('remove', i);
                },
                progressWidth() {
                    const doneLength = this.doneTasks.length;
                    const todoLength = this.todoTasks.length;
                    return `${doneLength / (doneLength + todoLength) * 100}%`
                }
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