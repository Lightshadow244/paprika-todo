<script lang="js">
	// @ts-nocheck
	import TextArea from './TextArea.svelte';
	import Input from './Input.svelte'
    import Checkbox from './Checkbox.svelte'

	import IconCheck from './icons/icon-check.svelte';
	import IconList from './icons/icon-list.svelte';
	import IconText from './icons/icon-text.svelte';
	import IconArrowDown from './icons/icon-arrow-down.svelte';
	import IconArrowUp from './icons/icon-arrow-up.svelte';
	import IconTrash from './icons/icon-trash.svelte';
	import IconSave from './icons/icon-save.svelte';
	import IconMore from './icons/icon-more.svelte';

	let { todo, ...props } = $props();

	let viewToDo = $state(true)
	let isAddNewItem = $state(false)

	let isCopied = $state(false)

	function updateTitle(event){
     	let title = event.target.value
        props.changeToDoTitle(todo.id, title)
	}

	function copyTodo(){
		let content = [] 
		todo.elements.forEach(element => {
			content.push(element.content)
		});

		navigator.clipboard.writeText(content.join("\n")).then(
			() => {
				isCopied = true
			},
			() => {
			/* clipboard write failed */
			},
		);
		window.setTimeout(() => {isCopied = false}, 1000);
	}

	
</script>

<section class="mx-2 my-2 mx-auto w-8/10 md:w-2xl lg:w-3xl xl:w-4xl">
	
	<div class="flex border-b font-bold p-1">
		<input class="w-full me-1" id="title" type="text" value={todo.title} onfocusout={(event) => {updateTitle(event)}}>
		<div class="btn btn-transparent ms-1 relative more-menu">
			<IconMore />
			<div class="absolute top-5 right-0 more-menu-item bg-slate-900 p-2 rounded w-32 border z-3">
				<!-- <button class="btn  w-full text-left p-1 {isCopied?"btn-emerald":"btn-transparent"}" onclick={() => {copyTodo()}}>Copy List</button> -->
				<button class="btn btn-transparent w-full text-left p-1 " onclick={() => {copyTodo()}}>{isCopied?"List copied":"Copy list"}</button>
				<div class="border-b" style="height:1px"></div>
				<button class="btn btn-transparent w-full text-left p-1" onclick={() => {props.clearElements(todo.id, false)}}>Clear</button>
				<button class="btn btn-transparent w-full text-left p-1" onclick={() => {props.clearElements(todo.id, true)}}>Clear Checks</button>
				<div class="border-b" style="height:1px"></div>
				<button class="btn btn-transparent w-full text-left p-1" onclick={() => {props.shuffleElements(todo.id)}}>Shuffle</button>
			</div>
		</div>
		<button	class="btn btn-emerald ms-1" onclick={() => {viewToDo = true}}><IconSave /></button>
	</div>
	

	<ul class=" mt-2" style="height: calc(100lvh - 6.5rem)">
		{#each todo.elements as element, index (index)}
			{#if element.type === "area"}
				<li class="mb-1 rounded bg-slate-800 relative element">
					<TextArea
									content={element.content}
									idTodo={todo.id}
									idElement={element.id}
									changeElementContent={props.changeElementContent}
								/>
					<div class="absolute right-0 top-0.5 element-controls grid grid-cols-3 h-7 gap-0.5">
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, 1)}}><IconArrowUp /></button>
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, -1)}}><IconArrowDown /></button>
						<button class="btn btn-rose h-full" onclick={() => {props.deleteElement(todo.id, element.id)}}><IconTrash /></button>
					</div>
				</li>
			{:else if element.type === "point"}
				
				<li class="mb-1 rounded bg-slate-800 relative element">
					<Input
									content={element.content}
									idTodo={todo.id}
									idElement={element.id}
									changeElementContent={props.changeElementContent}
								/>
					<div class="absolute right-0 top-0.5 element-controls grid grid-cols-3 h-7 gap-0.5">
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, 1)}}><IconArrowUp /></button>
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, -1)}}><IconArrowDown /></button>
						<button class="btn btn-rose h-full" onclick={() => {props.deleteElement(todo.id, element.id)}}><IconTrash /></button>
					</div>
				</li>
			{:else if element.type === "check"}
				<li class="mb-1 rounded bg-slate-800 relative element">
					<Checkbox
									content={element.content}
									idTodo={todo.id}
									idElement={element.id}
									check={element.check}
									changeElementContent={props.changeElementContent}
								/>
					<div class="absolute right-0 top-0.5 element-controls grid grid-cols-3 h-7 gap-0.5">
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, 1)}}><IconArrowUp /></button>
						<button class="btn btn-sky h-full" onclick={() => {props.changeElementPosition(todo.id, element.id, -1)}}><IconArrowDown /></button>
						<button class="btn btn-rose h-full" onclick={() => {props.deleteElement(todo.id, element.id)}}><IconTrash /></button>
					</div>
				</li>
			{/if}
		{/each}
		
		<div class="flex">
			<button class="btn-left btn-night mb-2 flex-1" onclick={() => {isAddNewItem = false;props.addTodoElement(todo.id, "point")}}><IconList /></button>
			<button class="btn-center btn-night  mb-2 flex-1" onclick={() => {isAddNewItem = false;props.addTodoElement(todo.id, "area")}}><IconText /></button>
			<button class="btn-right btn-night  mb-2 flex-1" onclick={() => {isAddNewItem = false;props.addTodoElement(todo.id, "check")}}><IconCheck /></button>
		</div>
		
		
		
	</ul>
        
	
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
	
	.btn-sky {
		@apply bg-sky-500 text-white;
	}
	.btn-sky:hover {
		@apply bg-sky-700 text-white;
	}

	.btn-emerald {
        @apply bg-emerald-500 text-white;
    }
    .btn-emerald:hover {
        @apply bg-emerald-700;
    }

	.btn-night {
		@apply  bg-slate-800;
	}
	.btn-night:hover {
		@apply bg-sky-500 text-white;
	}

	.btn-rose {
        @apply bg-rose-500 text-white;
    }
	.btn-rose:hover {
		@apply bg-rose-700;
	}

	.btn-transparent:hover {
		@apply bg-slate-800;
	}

	.element .element-controls{
		display: none;
	}

	.element:hover .element-controls{
		display: grid;
	}

	.more-menu .more-menu-item{
		display: none;
	}
	.more-menu:hover .more-menu-item{
		display: block;
	}
</style>
