<script lang="js">
	// @ts-nocheck
	import TextArea from './TextArea.svelte';
	import Input from './Input.svelte'
	let { todo, ...props } = $props();

	let viewToDo = $state(true)
	let isAddNewItem = $state(false)

	function updateTitle(event){
     	let title = event.target.value
        props.changeToDoTitle(todo.id, title)
	}

	
</script>

<section class="mx-2 my-2 md:mx-auto md:w-2xl lg:w-3xl xl:w-4xl">
	{#if viewToDo}
	<!-- View -->
	<div class="flex border-b font-bold p-1">
        <h3 class="">
      		{todo.title}
        </h3>
        
        <button	class="btn btn-sky ms-auto" onclick={() => {viewToDo = false}}>Edit</button>
	</div>
    	

        <ul class="mt-2" style="height: calc(100lvh - 6.5rem)">
            {#each todo.elements as element, index (index)}
                {#if element.type === "area"}
                    <li class="mb-1 rounded bg-slate-800 p-1 list-item-bullet">
                        <p class="whitespace-pre-line min-height-1">{element.content}</p>
                    </li>
                {:else if element.type === "point"}
                    <li class="mb-1 relative">
                        <div class="absolute -left-4 p-1 prevent-select">&#8226;</div>
                        <p class="rounded bg-slate-800 whitespace-pre-line min-height-1 p-1">{element.content}</p>
                    </li>
                {:else if element.type === "check"}
                    <li class="mb-1 rounded bg-slate-800 p-1">
                        <p class="whitespace-pre-line min-height-1">{element.content}</p>
                    </li>
                {/if}
            {/each}
        </ul>
	{:else}
	<!-- Edit -->
	    <div class="flex border-b font-bold p-1">
			<input class="w-full me-1" id="title" type="text" value={todo.title} onfocusout={(event) => {updateTitle(event)}}>
         
       		<button	class="btn btn-emerald ms-1" onclick={() => {viewToDo = true}}>Save</button>
		</div>
        

        <ul class=" mt-2" style="height: calc(100lvh - 6.5rem)">
            {#each todo.elements as element, index (index)}
                {#if element.type === "area"}
                    <li class="mb-1 rounded bg-slate-800 p-1">
                        <TextArea
                       					content={element.content}
                       					idTodo={todo.id}
                       					idElement={element.id}
                       					changeElementContent={props.changeElementContent}
                    				/>
                    </li>
                {:else if element.type === "point"}
                    
                    <li class="mb-1 rounded bg-slate-800 p-1 relative">
                        <div class="absolute -left-4 prevent-select">&#8226;</div>
                        <Input
                       					content={element.content}
                       					idTodo={todo.id}
                       					idElement={element.id}
                       					changeElementContent={props.changeElementContent}
                    				/>
                    </li>
                {:else if element.type === "check"}
                    <li class="mb-1 rounded bg-slate-800 p-1">
                        <p class="whitespace-pre-line min-height-1">{element.content}</p>
                    </li>
                {/if}
            {/each}
            
            {#if isAddNewItem}
                <div class="flex">
                    <button class="btn-left btn-night mb-2 flex-1" onclick={() => {props.addTodoElement(todo.id, "point")}}>point</button>
                    <button class="btn-center btn-night  mb-2 flex-1" onclick={() => {props.addTodoElement(todo.id, "area")}}>area</button>
                    <button class="btn-right btn-night  mb-2 flex-1" onclick={() => {props.addTodoElement(todo.id, "check")}}>check</button>
                </div>
            {:else}
                <button class="btn-add-item btn-night mb-2 w-full" onclick={() => {isAddNewItem = true}}>+</button>
                
            {/if}
            
            
        </ul>
        
	{/if}
</section>


<style>
	@reference "../routes/layout.css";

	.btn {
		@apply rounded px-1 text-sm;
	}

	.btn-left{
	    @apply rounded-l text-sm font-bold p-1 h-8;
	}

	.btn-center{
	    @apply  text-sm font-bold p-1 h-8;
	}

	.btn-right{
	    @apply rounded-r text-sm font-bold p-1 h-8;
	}

	.btn-add-item{
	    @apply rounded font-bold p-1 h-8;
	}
	
	.btn-sky {
		@apply bg-sky-500 text-white;
	}
	.btn-sky:hover {
		@apply bg-sky-700 text-white;
	}

	.btn-emerald {
        @apply bg-emerald-500 ;
    }
    .btn-emerald:hover {
        @apply bg-emerald-700 text-white;
    }

	.btn-night {
		@apply  bg-slate-800;
	}
	.btn-night:hover {
		@apply bg-sky-500 text-white;
	}

	.min-height-1{
	    min-height: 1rem;
	}
	.prevent-select {
        -webkit-user-select: none; /* Safari */
        -ms-user-select: none; /* IE 10 and IE 11 */
        user-select: none; /* Standard syntax */
    }
</style>
