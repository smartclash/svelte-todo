<svelte:head>
	<link rel="stylesheet" href="https://unpkg.com/todomvc-app-css@2.2.0/index.css">
</svelte:head>

<style>
	.hide {
		display: none !important;
	}
</style>

<script>
	import TodoItem from './Components/TodoItem.svelte';
	import { todos } from './store.js';

	let todoID = 1;
	let todo = '';

	function addTodo(event) {
		if (event.keyCode !== 13) {
			return false;
		}

		if (!todo.trim()) {
			return false;
		}

		todos.set([...$todos, {
			id: todoID,
			title: todo,
			completed: false,
			editing: false
		}]);
		console.log(todo, $todos);
		todo = '';
		todoID = todoID + 1;
	}
</script>

<section class="todoapp">
	<header class="header">
		<h1>todos</h1>
		<input class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?" on:keyup={addTodo} bind:value={todo}>
	</header>
	<section class="main" >
		<input id="toggle-all" class="toggle-all" type="checkbox">
		<label for="toggle-all" class:hide="{$todos.length === 0}"></label>
		<ul class="todo-list">
			{#each $todos as list}
				<TodoItem {...list} />
			{/each}
		</ul>
	</section>
	<!-- <footer class="footer" v-show="todos.length" v-cloak>
		<span class="todo-count">
			<strong>{{ remaining }}</strong> {{ remaining | pluralize }} left
		</span>
		<ul class="filters">
			<li><a href="#/all" :class="{ selected: visibility == 'all' }">All</a></li>
			<li><a href="#/active" :class="{ selected: visibility == 'active' }">Active</a></li>
			<li><a href="#/completed" :class="{ selected: visibility == 'completed' }">Completed</a></li>
		</ul>
		<button class="clear-completed" @click="removeCompleted" v-show="todos.length > remaining">
			Clear completed
		</button>
	</footer> -->
</section>
<footer class="info">
	<p>Double-click to edit a todo</p>
	<p>Written by <a href="http://evanyou.me">Evan You</a></p>
	<p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
</footer>
