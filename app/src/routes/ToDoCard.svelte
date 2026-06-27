<script lang="js">
    let {todo, selectTodo, deleteTodo, ...props} = $props()

</script>
<div class="relative">
    <button class="rounded border-1 hover:shadow-md hover:shadow-slate-500 hover:cursor-pointer w-full" onclick={() => selectTodo(todo.id)}>
        <h3 class="m-1 font-bold border-b">{todo.header}</h3>
        {#each todo.elements as element, index (index)}
            <article class="m-1">{element.content}</article>
        {/each}
    
    </button>
    <button class="btn btn-rose absolute top-1 right-1" onclick={() => {deleteTodo(todo.id)}}>Del</button>
</div>


<style>
    @reference "../routes/layout.css";

    .btn {
        @apply font-bold px-2 rounded font-bold;
    }
    .btn-rose {
        @apply bg-rose-500 text-white;
    }
    .btn-rose:hover {
        @apply bg-rose-700 text-white;
    }

    .btn-border-rose {
        @apply border-2 border-rose-500;
    }
    .btn-border-rose:hover {
        @apply bg-rose-700 border-rose-600 text-white;
    }
</style>
