<template>
	<AddTask v-show="showAddTask" @add-task="addTask" />
	<Tasks
		@toggle-reminder="toggleReminder"
		@delete-task="deleteTask"
		:tasks="tasks"
	/>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

export default defineComponent({
	name: "App",
	props: {
		showAddTask: Boolean,
	},
	components: {
		Tasks,
		AddTask,
	},
	data() {
		return {
			tasks: [],
		};
	},
	methods: {
		async addTask(task: any) {
			const res = await fetch("api/tasks", {
				method: "POST",
				headers: {
					"Content-type": "application/json",
				},
				body: JSON.stringify(task),
			});
			const data = await res.json();
			this.tasks = [...this.tasks, data];
		},
		async deleteTask(id: number) {
			if (confirm("Are you sure?")) {
				const res = await fetch(`api/tasks/${id}`, {
					method: "DELETE",
					headers: {
						"Content-type": "application/json",
					},
				});
				res.status === 200
					? (this.tasks = this.tasks.filter((task: any) => task.id !== id))
					: alert("Error Deleting Task");
			}
		},
		async toggleReminder(id: number) {
			const taskToToggle = await this.fetchTask(id);
			const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

			const res = await fetch(`api/tasks/${id}`, {
				method: "PUT",
				headers: {
					"Content-type": "application/json",
				},
				body: JSON.stringify(updateTask),
			});

			const data = await res.json();

			this.tasks = this.tasks.map((task: any) =>
				task.id === id ? { ...task, reminder: data.reminder } : task
			);
		},
		async fetchTasks() {
			const res = await fetch("api/tasks");
			const data = await res.json();
			return data;
		},
		async fetchTask(id: number) {
			const res = await fetch(`api/tasks/${id}`);
			const data = await res.json();
			return data;
		},
	},
	async created() {
		this.tasks = await this.fetchTasks();
	},
});
</script>
