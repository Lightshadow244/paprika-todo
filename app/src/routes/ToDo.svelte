<script lang="js">
// @ts-nocheck
    import TextArea from "./TextArea.svelte";
    let {todo, ...props} = $props()
</script>

<section class="md:w-2xl lg:w-3xl xl:w-4xl mx-2 md:mx-auto my-2">
    <h3 class="font-bold text-lg m-1 border-b">{todo.header}</h3>
    <ul class=" mt-2 list-disc" style="height: calc(100lvh - 6.5rem)">
        {#each todo.elements as element, index (index)}
            <li class="bg-slate-800 rounded p-1 mb-1">
                <!-- <textarea class="w-full" cols="1" rows="1">{element.content}</textarea> -->
                <TextArea content={element.content} idTodo={todo.id} idElement={element.id} changeElementContent={props.changeElementContent}/>
            </li>
        {/each}
        <button class="btn-add-item" onclick={() => props.addTodoElement(todo.id)}>+</button>
    </ul>
</section>

<style>
    @reference "../routes/layout.css";

    .btn-add-item{
        @apply bg-slate-800 rounded p-1 w-full font-bold;
    }
    .btn-add-item:hover {
        @apply bg-sky-500 text-white;
    }

</style>
