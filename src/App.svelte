<style>
	.hide {
		display: none !important;
	}
</style>

<script>
	import TodoItem from './Components/TodoItem.svelte'
	import { todos } from './store.js';

	let todoID = 1;
	let todo = '';

	$: remainingTodos = $todos.filter(element => !element.completed).length;

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

		todo = '';
		todoID = todoID + 1;
	}

	function toggleTodoStatus({detail : { id }}) {
		let tempTodo = $todos;

		for(let i in $todos) {
			if (tempTodo[i].id === id) {
				tempTodo[i].completed = !tempTodo[i].completed
			}
		}

		todos.set(tempTodo);
	}

	function deleteTodo({detail : { id }}) {
		let tempTodo = $todos;
		todos.set(tempTodo.filter(element => element.id !== id));
	}

	function clearCompleted() {
		let tempTodo = $todos;
		todos.set(tempTodo.filter(element => !element.completed));
	}

	function editTodo({ detail: { id }}) {
		let tempTodo = $todos;

		for(let i in $todos) {
			if (tempTodo[i].id === id) {
				tempTodo[i].editing = !tempTodo[i].editing
			}
		}

		todos.set(tempTodo);
	}

	// ES6 Destructuring to get keyCode and value from form via native DOM events
	function saveEditedTodo({ 
		detail: { 
			id, 
			event: {
				keyCode,
				target: { value }
			}
		}
	}) {
		if (keyCode != 13) {
			return false;
		}

		let tempTodo = $todos;

		for(let i in $todos) {
			if (tempTodo[i].id === id) {
				tempTodo[i].title = value;
				tempTodo[i].editing = false;
			}
		}

		todos.set(tempTodo);
	}
</script>

<section class="todoapp">
	<header class="header">
		<h1>todos</h1>
		<input class="new-todo" autocomplete="off" placeholder="What needs to be done?" on:keyup={addTodo} bind:value={todo}>
	</header>
	<section class="main">
		<input id="toggle-all" class="toggle-all" type="checkbox">
		<label for="toggle-all" class:hide="{$todos.length === 0}"></label>
		<ul class="todo-list">
			{#each $todos as list}
				<TodoItem 
					{...list} 
					on:completed={toggleTodoStatus}
					on:delete={deleteTodo}
					on:editing={editTodo}
					on:edited={saveEditedTodo} />
			{/each}
		</ul>
	</section>
	<footer class="footer" class:hide="{$todos.length === 0}">
		<span class="todo-count">
			<strong>{ remainingTodos }</strong> { remainingTodos > 1 ? 'todos' : 'todo' } left
		</span>
		<button class="clear-completed" on:click={clearCompleted}>
			Clear completed
		</button>
	</footer>
</section>
<footer class="info">
	<p>Double-click to edit a todo</p>
	<p>Written by <a href="https://alphaman.me">xXAlphaManXx</a></p>
</footer>
