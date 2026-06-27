<script lang="js">
// @ts-nocheck

import Selection from "./Selection.svelte"
import ToDo from "./ToDo.svelte"
	import { onMount } from 'svelte';

// let isTodoSelected = $state(false)
let selectedTodo = $state(-1)
// let todos = $state([{
//         "id": 0,
//         "header": "Dies ist ein Todo", 
//         "elements": [{
//             "id": 0,
//             "type": "checkbox", 
//             "content":"Dies ist der erste Content mit einer Checkbox"
//         }]
//     },
//     {
//         "id": 1,
//         "header": "Dies ist das zweite Todo", 
//         "elements": [{
//             "id": 1,
//             "type": "checkbox", 
//             "content":"Dies ist der Content des zweiten Todos mit einer Checkbox"
//         }]
//     }]
// )
let todos = $state([])
// let todos = $state(JSON.parse(getCookie("todos")))

function addTodo(){
    todos.push({
        "id":todos.length,
        "header": "Title", 
        "elements": [{
            "id":0,
            "type": "text", 
            "content":"Content"
        }]
    })
    updateCookie()
}

function addTodoElement(idTodo){
    todos[idTodo].elements.push({
            "id": todos[idTodo].elements.length,
            "type": "text", 
            "content":""
        })
    updateCookie()
}

function changeElementContent(idTodo, idElement, content){
    todos[idTodo].elements[idElement].content = content
    updateCookie()
}

function selectTodo(id){
    selectedTodo = id
}

function deleteTodo(id){
    todos.splice(id,1)
    
    let index = 0
    todos.forEach(element => {
        element.id = index
        index++
    });
    updateCookie()
}

function updateCookie(){
    let age = 60 * 60 * 24 * 365
    document.cookie = "todos=" + JSON.stringify(todos) + "; max-age=" + age + "; path=/;"
}

function getCookie(name) {
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) return parts.pop().split(';').shift();
}

// if (document.cookie.indexOf(";") > -1) {
    
    
// }else{
//     document.cookie = "todo=" + JSON.stringify(todos)
// }


onMount(() => {
        let rawTodos = getCookie("todos")
        if(rawTodos){
            todos = JSON.parse(rawTodos)
        }
		
});
</script>


<header class="border-b p-1 flex items-center">
    <h2 class="font-bold text-xl">Paprika-ToDo</h2>
    {#if selectedTodo == -1}
        <button class="btn btn-border-emerald ms-2" onclick={() => addTodo()}>+</button>
    {:else}
        <button class="btn btn-border-rose ms-2" onclick={() => selectTodo(-1)}>Back</button>
    {/if}
</header>

{#if selectedTodo > -1}
    <ToDo todo={todos[selectedTodo]} addTodoElement={addTodoElement} changeElementContent={changeElementContent}/>
{:else}
    <Selection todos={todos} selectTodo={selectTodo} deleteTodo={deleteTodo}/>
{/if}

<style>
@reference "../routes/layout.css";

    .btn {
        @apply font-bold px-2 rounded;
    }
    /* Emerald */
    .btn-emerald {
        @apply bg-emerald-500 ;
    }
    .btn-emerald:hover {
        @apply bg-emerald-700 text-white;
    }

    .btn-border-emerald {
        @apply border-2 border-emerald-500;
    }
    .btn-border-emerald:hover {
        @apply bg-emerald-700 border-emerald-600 text-white;
    }

    /* Rose */
    .btn-rose {
        @apply bg-rose-500 text-white;
    }
    .btn-border-rose {
        @apply border-2 border-rose-500;
    }
    .btn-border-rose:hover {
        @apply bg-rose-700 text-white;
    }

    /* Sky */
    .btn-sky {
        @apply bg-sky-500 text-white;
    }
    .btn-sky:hover {
        @apply bg-sky-700 text-white;
    }

</style>



