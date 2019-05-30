<script>
    import  { createEventDispatcher } from 'svelte';

    export let title;
    export let id;
    export let completed;
    export let editing;

    const dispatch = createEventDispatcher();

    const dispatchEdit = () => dispatch('editing', { id });
    const dispatchDelete = () => dispatch('delete', { id });
    const dispatchEditDone = (event) => dispatch('edited', { id, event });
    const dispatchCompleted = () => dispatch('completed', { id });
</script>

<li class="todo" class:completed={completed} class:editing={editing}>
    <div class="view">
        <input class="toggle" type="checkbox" bind:checked={completed} on:click={dispatchCompleted}>
        <label on:dblclick={dispatchEdit}>{ title }</label>
        <button class="destroy" on:click={dispatchDelete}></button>
    </div>
    <input class="edit" type="text" on:keyup={dispatchEditDone} value={title}>
</li>
