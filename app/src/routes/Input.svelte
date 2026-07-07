<script lang="js">
	import { onMount } from "svelte";


// @ts-nocheck
    let {content, idTodo, idElement, ...props} = $props()
    /*** @type {HTMLInputElement}*/ 
    let input
    
    function updateContent(){
      props.changeElementContent(idTodo, idElement, input.value)
    }

</script>
<div class="relative">
    <div class="absolute -left-5 prevent-select">&#8226;</div>
    <input bind:this={input} class="w-full courier-font p-1" onfocusout={() => updateContent()} value={content}>
</div>

<style>
    .courier-font{
        font-family: 'Courier New', Courier, monospace;
    }
</style>