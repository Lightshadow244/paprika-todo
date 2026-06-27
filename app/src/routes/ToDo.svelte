<script lang="js">
// @ts-nocheck
    import TextArea from "./TextArea.svelte";
    let {todo, ...props} = $props()

    let editHeader = $state(false)

    function changeEditHeader(status){
        editHeader = status

        if (status == true) {
            
        }
    }
</script>

<section class="md:w-2xl lg:w-3xl xl:w-4xl mx-2 md:mx-auto my-2">
    {#if editHeader}
        <input class="font-bold text-lg m-1 border-b w-full" type="text" value={todo.header} onfocusout={() => {changeEditHeader(false)}}>
    {:else}
        <h3 class="font-bold text-lg m-1 border-b relative">
            {todo.header}
            <button class="btn btn-sky absolute right-1 top-1" onclick={() => {changeEditHeader(true)}}>edit</button>
        </h3>
    {/if}

    

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

    .btn {
        @apply px-1 rounded text-sm;
    }
    .btn-sky {
        @apply bg-sky-500 text-white;
    }
    .btn-sky:hover {
        @apply bg-sky-700 text-white;
    }

    .btn-add-item{
        @apply bg-slate-800 rounded p-1 w-full font-bold;
    }
    .btn-add-item:hover {
        @apply bg-sky-500 text-white;
    }

</style>
