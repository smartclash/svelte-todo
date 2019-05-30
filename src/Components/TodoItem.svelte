<script>
    import  { createEventDispatcher } from 'svelte';

    export let title;
    export let id;
    export let completed;
    export let editing;

    const dispatch = createEventDispatcher();

    function handleDelete() {

    }

    function handleEdit() {

    }

    function dispatchCompleted() {
        return dispatch('completed', {
            id
        });
    }
</script>

<li class="todo" class:completed={completed} class:editing={editing}>
    <div class="view">
        <input class="toggle" type="checkbox" on:click={dispatchCompleted}>
        <label>{ title }</label>
        <button class="destroy" @click="removeTodo(todo)"></button>
    </div>
    <input class="edit" type="text" v-model="todo.title" v-todo-focus="todo == editedTodo" @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
</li>
