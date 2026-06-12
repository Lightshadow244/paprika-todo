<script lang="js">
// @ts-nocheck
    let {content, idTodo, idElement, ...props} = $props()
    let textarea

    function updateContent(event){
        if (event.type === "keypress") {
            if (event.key === "Enter") {
            //   props.changeElementContent(idTodo, idElement, textarea.value)  
            }
        }else if(event.type === "focusout"){
            props.changeElementContent(idTodo, idElement, textarea.value)  
        }

        
    }
</script>
<textarea bind:this={textarea} class="w-full" cols="1" rows="1" onkeypress={(event) => updateContent(event)} onfocusout={(event) => updateContent(event)}>{content}</textarea>