<!-- TaskListComponent.vue -->
<template>
    <div>
        <h1>Task Manager</h1>

        <!-- Display tasks -->
        <div v-if="2">
            <h2>Tasks</h2>
            <ul>
                <li v-for="task in tasks" :key="task.id">
                    
                    {{ task.title }} - {{ task.description }}
                    <button @click="updateTask(task.id, { title: 'Updated Title', description: 'Updated Description' })">
                        Update
                    </button>
                    <button @click="deleteTask(task.id)">Delete</button>
                </li>
            </ul>
        </div>
        <div v-else>
            <p>No tasks available.</p>
        </div>
        <!-- Create new task -->
        <div>
            <h2>Create New Task</h2>
            <input type="text" v-model="newTask.title" placeholder="Task Title">
            <input type="text" v-model="newTask.description" placeholder="Task Description">
            <button @click="createTask">Create Task</button>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {

    data() {
        return {
            tasks: [],
            newTask: {
                title: '',
                description: ''
            }
        };
    },


    methods: {
        async getAllTasks() {
            try {
                const response = await axios.get(process.env.VUE_APP_API_BASE_URL + '/tasks');
                this.tasks= response.data.tasks;
                //console.log(response.data);
            } catch (error) {
                console.error('Error fetching tasks:', error);
            }
        },
        async createTask() {
            try {
                await axios.post(process.env.VUE_APP_API_BASE_URL + '/tasks', this.newTask);
                this.getAllTasks(); // Refresh task list after creating a new task
                this.newTask = { title: '', description: '' }; // Clear input fields
            } catch (error) {
                console.error('Error creating task:', error);
            }
        },
        async updateTask(taskId, updatedTask) {
            try {
                await axios.put(process.env.VUE_APP_API_BASE_URL + `/tasks/${taskId}`, updatedTask);
                this.getAllTasks(); // Refresh task list after updating a task
            } catch (error) {
                console.error('Error updating task:', error);
            }
        },
        async deleteTask(taskId) {
            try {
                await axios.delete(process.env.VUE_APP_API_BASE_URL + `/tasks/${taskId}`);
                this.getAllTasks(); // Refresh task list after deleting a task
            } catch (error) {
                console.error('Error deleting task:', error);
            }
        }
    },
    created() {
        this.getAllTasks();
        //  const baseUrl = process.env.VUE_APP_API_BASE_URL;
        // const apiKey = process.env.VUE_APP_API_KEY;
        // Fetch tasks when component is created
    }
};
</script>
  
<style scoped>
/* Your component styles */
</style>
  