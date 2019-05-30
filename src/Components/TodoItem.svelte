<script>
    export let title;
    export let id;
    export let completed;
    export let editing;
</script>

<li class="todo" class:completed={completed}>
    <div class="view">
        <input class="toggle" type="checkbox" bind:checked={completed}>
        <label>{ title }</label>
        <button class="destroy" @click="removeTodo(todo)"></button>
    </div>
    <!-- <input class="edit" type="text" v-model="todo.title" v-todo-focus="todo == editedTodo" @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)"> -->
</li>
