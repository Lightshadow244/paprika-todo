<script lang="js">
    let {todo, selectTodo, ...props} = $props()

</script>

<button class="rounded border-1 hover:shadow-md hover:shadow-slate-500 hover:cursor-pointer" onclick={() => selectTodo(todo.id)}>
    <h3 class="m-1 font-bold border-b">{todo.header}</h3>
    {#each todo.elements as element, index (index)}
        <article class="m-1">{element.content}</article>
    {/each}
</button>
