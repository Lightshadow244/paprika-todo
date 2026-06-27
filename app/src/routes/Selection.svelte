<script lang="js">
// @ts-nocheck

    import ToDoCard from "./ToDoCard.svelte";

    let {todos, selectTodo, deleteTodo, ...props} = $props()
</script>

<grid class="grid grid-cols-3 gap-4 md:w-2xl lg:w-3xl xl:w-4xl mx-2 md:mx-auto my-2">
    {#each todos as todo, index (index)}
        <ToDoCard todo={todo} selectTodo={selectTodo} deleteTodo={deleteTodo}/>
    {/each}
</grid>