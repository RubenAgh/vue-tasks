<template>
    <div>
        <h2>List</h2>
        <div class="row">
            <div class="input-field col s6">
                <select ref="select" v-model="filter">
                    <option value="" disabled selected>Choose your option</option>
                    <option value="all">All</option>
                    <option value="active">Active</option>
                    <option value="completed">Completed</option>
                </select>    
                <label>Status filter</label>
            </div>
        </div>

        <hr>
        <table v-if="tasksFilter.length">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Title</th>
                    <th>Date</th>
                    <th>Description</th>
                    <th>Status</th>
                    <th>Open</th>
                </tr>
            </thead>
            <tbody>
                <tr 
                    v-for="(task, index) of tasksFilter"
                    :key="task.id"
                >
                    <td>{{ index + 1 }}</td>
                    <td>{{ task.title }}</td>
                    <td>{{ new Date(task.date).toLocaleDateString() }}</td>
                    <td class="descriptionTd">
                        <div class="descriptionText">{{ task.description }}</div>
                    </td>
                    <td>{{ task.status }}</td>
                    <td>
                        <router-link :to="'/task/' + task.id" tag="button" class="btn btn-small">
                            Open
                        </router-link>
                    </td>
                </tr>
            </tbody>
        </table>
        <p v-else>No tasks</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                filter: null
            }
        },
        computed: {
            tasks() {
                return this.$store.getters.tasks;
            },
            tasksFilter() {
                return this.tasks.filter(task => {
                    if (!this.filter || this.filter === 'all') {
                        return true;
                    }
                    return task.status === this.filter;
                });
            }
        },
        mounted() {
            M.FormSelect.init(this.$refs.select);
        },
    }
</script>

<style scoped>
    .descriptionText {
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    .descriptionTd {
        max-width: 400px;
    }
</style>