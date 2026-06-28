<script lang="js">
	import { onMount } from "svelte";


// @ts-nocheck
    let {content, idTodo, idElement, ...props} = $props()
    let textarea
    let rows = $state(0)
    
    function updateContent(){
      props.changeElementContent(idTodo, idElement, textarea.value)
    }

    function countRows(){
      let tmpRows = 0
      let charWidth = 10 
      let maxCountChars = textarea.clientWidth / charWidth
      
      let textSections = content.split("\n")
  
      for(let i = 0; i<textSections.length; i++){
        tmpRows += 1 // new line
        let text = textSections[i]
        let sectionRows = Math.round(text.length / maxCountChars)
        tmpRows += sectionRows
      }

      if(tmpRows == 0){
        rows = 1
      }else{
        rows = tmpRows
      }
    } 

    onMount(() => {
            countRows()
		
    });
</script>
<textarea bind:this={textarea} class="w-full courier-font" cols="1" rows={rows} onfocusout={() => updateContent()}>{content}</textarea>

<style>
    .courier-font{
        font-family: 'Courier New', Courier, monospace;
    }
</style>