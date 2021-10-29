<script>
	import TodoItem from './TodoItem.svelte';

	let newTodoTitle = '';
	let currentFilter = 'all';
	let nextId = 4;

	let todos = [
		{
			id: 1,
			title: 'My first todo',
			completed: false,
		},
		{
			id: 2,
			title: 'My second todo',
			completed: false,
		},
		{
			id: 3,
			title: 'My third todo',
			completed: false,
		}
	];

	function addTodo(event) {
		if(event.key === 'Enter') {
			todos = [ ...todos, {
				id: nextId,
				completed: false,
				title: newTodoTitle,
			}];

			nextId = nextId + 1;
			newTodoTitle = '';
		}
	}

	$: todosRemaining = filterTodos.filter(todo => !todo.completed).length
	$: filterTodos = currentFilter === 'all' ? todos : currentFilter === 'completed' 
		? todos.filter(todo => todo.completed) 
		: todos.filter(todo => !todo.completed)

	function checkAllTodos(event) {
		todos.forEach(todo => todo.completed = event.target.checked);
		todos = todos;
	}

	function updateFilter(newFilter) {
		currentFilter = newFilter;
	}

	function clearCompleted() {
		todos = todos.filter(todo => !todo.completed);
	}

	function handleDeleteTodo(event) {
		todos = todos.filter(todo => todo.id !== event.detail.id);
	}

	function handleToggleComplete(event) {
		const todoIndex = todo.findIndex(todo => todo.id === event.detail.id);
		const updateTodo = { ...todos[todoIndex], completed: !todos[indexIndex].completed};

		todos = [
			...todos.slice(0, todoIndex),
			updateTodo,
			...todos.slice(todoIndex+1)
		];
	}

</script>

<style>

	.todo-input{
		width: 100%;
		border: 1px solid #ccc;
		border-radius: 5px;
		outline: none;
		padding: 15px;
	}

	.container {
			max-width: 800px;
			margin: 10px auto;
	}

	.todo-item{
		margin-top: 30px;
	}

	.inner-container {
			display: flex;
			align-items: center;
			justify-content: space-between;
			font-size: 16px;
			border-top: 1px solid lightgrey;
			padding-top: 15px;
			margin-bottom: 13px;
	}
	
	.inner-container-input {
			margin-right: 12px;
	}
	button {
			font-size: 16px;
			background-color: white;
			appearance: none;
			border: 1px solid lightseagreen;
			padding: 5px 15px;
	}

	button:hover {
			background: lightseagreen;
			border: 1px solid lightseagreen;
	}
	button:focus {
			outline: none;
	}
	.active {
			background: lightseagreen;
	}


</style>

<div class="container">
	<h1>Todo List</h1>
	<input type="text" class="todo-input" placeholder="輸入待辦事項" bind:value={newTodoTitle} on:keydown={addTodo}>

	{#each filterTodos as todo}
		<div class="todo-item">
			<TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
		</div>
	{/each}

	<div class="inner-container">
		<div><label><input class="inner-container-input" type="checkbox" on:change={checkAllTodos}>Check All</label></div>
		<div>{todosRemaining} items left</div>
	</div>

	<div class="inner-container">
		<div>
			<button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
			<button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
			<button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">completed</button>
		</div>
		<div>
			<button on:click={clearCompleted}>Clear completed</button>
		</div>
	</div>

</div>