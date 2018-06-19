<template>
    <div>
        <div class="row">
            <div class="col-md-offset-3 col-md-6">
                <h3>New Task</h3>
                <div class="input-group">
                    <input v-model="task" class="form-control" maxlength="12">
                    <span class="input-group-btn">
                        <button class="btn btn-primary"
                                @click="addTask(task)">
                            Add
                        </button>
                        <button class="btn btn-danger"
                                @click="removeAll">
                            Clear All
                        </button>
                    </span>
                </div>
                <div class="label label-warning"
                     :style="{opacity: taskError ? 1 : 0, transition: '0.3s'}">
                    {{errorMsg}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'AddTask',
        props: ['taskError'],
        data() {
            return {
                task: '',
                errorMsg: 'Pole nie może się powtarzać lub być pustę'
            }
        },
        methods: {
            addTask(task = '') {
                this.$emit('addTask', task);
                this.task = '';
            },
            removeAll() {
                this.$emit('removeAll');
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>