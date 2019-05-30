<script>
    import  { createEventDispatcher } from 'svelte';

    export let title;
    export let id;
    export let completed;
    export let editing;

    const dispatch = createEventDispatcher();

    const dispatchDelete = () => dispatch('delete', { id });
    const dispatchEditing = () => dispatch('editing', { id });
    const dispatchCompleted = () => dispatch('completed', { id });
</script>

<li class="todo" class:completed={completed} class:editing={editing}>
    <div class="view">
        <input class="toggle" type="checkbox" bind:checked={completed} on:click={dispatchCompleted}>
        <label on:dblclick={dispatchEditing}>{ title }</label>
        <button class="destroy" on:click={dispatchDelete}></button>
    </div>
    <input class="edit" type="text" v-model="todo.title" v-todo-focus="todo == editedTodo" @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
</li>
