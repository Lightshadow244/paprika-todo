<script lang="js">
	import { onMount } from "svelte";


// @ts-nocheck
    let {content, idTodo, idElement, check, ...props} = $props()
    /*** @type {HTMLInputElement}*/ 
    let checkbox
    /*** @type {HTMLInputElement}*/ 
    let input 
    
    function updateContent(){
      props.changeElementContent(idTodo, idElement, input.value, checkbox.checked)
    }

    onMount(() => {
        checkbox.checked=check
		
    })

    $effect(() => {
        checkbox.checked=check
    })

</script>

<div class="relative">
    <input bind:this={checkbox} class="absolute -left-5 prevent-select my-auto top-0 bottom-0" type="checkbox" id="checkbox-{idElement}" name="checkbox-{idElement}" onclick={() => updateContent()}>

    <input bind:this={input} class="w-full courier-font p-1" onfocusout={() => updateContent()} value={content}>
</div>

<style>
    .courier-font{
        font-family: 'Courier New', Courier, monospace;
    }
</style>