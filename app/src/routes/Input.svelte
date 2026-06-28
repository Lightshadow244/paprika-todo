<script lang="js">
	import { onMount } from "svelte";


// @ts-nocheck
    let {content, idTodo, idElement, ...props} = $props()
    let input
    
    function updateContent(){
      props.changeElementContent(idTodo, idElement, input.value)
    }

</script>
<input bind:this={input} class="w-full courier-font" onfocusout={() => updateContent()} value={content}>

<style>
    .courier-font{
        font-family: 'Courier New', Courier, monospace;
    }
</style>