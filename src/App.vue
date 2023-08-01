<template>
	<div class="container">
		<Header
			@toggle-add-task="toggleAddTask"
			title="Task Tracker"
			:showAddTask="showAddTask"
		/>
    <router-view :showAddTask="showAddTask"></router-view>
    <Footer/>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";


export default defineComponent({
	name: "App",
	components: {
		Header,
    	Footer,
	},
	data() {
		return {
			showAddTask: true,
		};
	},
	methods: {
		toggleAddTask() {
			this.showAddTask = !this.showAddTask;
		}
	}
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&family=Source+Code+Pro:ital,wght@0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

* {
	box-sizing: border-box;
	margin: 0;
	padding: 0;
}

body {
	font-family: "REM", sans-serif;
}

.container {
	max-width: 500px;
	margin: 30px auto;
	overflow: auto;
	min-height: 300px;
	border: 1px solid steelblue;
	padding: 30px;
	border-radius: 5px;
}

.btn {
	display: inline-block;
	background: #000;
	color: #fff;
	border: none;
	padding: 10px 20px;
	margin: 5px;
	border-radius: 5px;
	cursor: pointer;
	text-decoration: none;
	font-size: 15px;
	font-family: inherit;
}

.btn:focus {
	outline: none;
}

.btn:active {
	transform: scale(0.98);
}

.btn-block {
	display: block;
	width: 100%;
}
</style>
