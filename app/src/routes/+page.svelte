<script lang="js">
// @ts-nocheck

import Selection from "./Selection.svelte"
import ToDo from "./ToDo.svelte"
import { onMount } from 'svelte';

let selectedTodo = $state(-1)
let todos = $state([])


function addTodo(){
    let id = todos.length
    todos.push({
        "id":id,
        "title": "New List", 
        "elements": []
    })
    updateCookie()
    selectedTodo = id
}

function addTodoElement(idTodo, type){
    todos[idTodo].elements.push({
            "id": todos[idTodo].elements.length,
            "type": type, 
            "content":"",
            "check": false
        })
    updateCookie()
}

function changeElementContent(idTodo, idElement, content, check=false){
    todos[idTodo].elements[idElement].content = content
    todos[idTodo].elements[idElement].check = check
    updateCookie()
}

function changeElementPosition(idTodo, idElement, positionDelta){
    let element = todos[idTodo].elements[idElement]
    let toIndex = element.id - positionDelta
    let fromIndex = element.id

    todos[idTodo].elements.splice(fromIndex, 1);
    todos[idTodo].elements.splice(toIndex, 0, element);

    let index = 0
    todos[idTodo].elements.forEach(element => {
        element.id = index
        index++
    });
    updateCookie()
}

function deleteElement(idTodo, idElement){
    todos[idTodo].elements.splice(idElement,1)
    
    let index = 0
    todos[idTodo].elements.forEach(element => {
        element.id = index
        index++
    });
    updateCookie()
}

function shuffleElements(idTodo){
    let currentIndex = todos[idTodo].elements.length;

    while (currentIndex != 0) {

        let randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        [todos[idTodo].elements[currentIndex], todos[idTodo].elements[randomIndex]] = [
        todos[idTodo].elements[randomIndex], todos[idTodo].elements[currentIndex]];
    }
    updateCookie()
}

function clearElements(idTodo, onlyChecks){
    if (onlyChecks) {
        let tmpChecks = []
        todos[idTodo].elements.forEach(element => {
            if (element.check) {
                tmpChecks.push(element.id)
            }
        });


        tmpChecks.slice().reverse().forEach(id => {
            todos[idTodo].elements.splice(id,1)
        })
        
        let index = 0
        todos[idTodo].elements.forEach(element => {
            element.id = index
            index++
        });
    
    }else{
        todos[idTodo].elements = []
    }
    updateCookie()
}

function changeToDoTitle(idTodo, title){
  todos[idTodo].title = title
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


onMount(() => {
        let rawTodos = getCookie("todos")
        if(rawTodos){
            todos = JSON.parse(rawTodos)
        }
		
});
</script>

<header class="border-b p-1 flex items-center">
    <!-- svelte-ignore a11y_invalid_attribute -->
    <a class="font-bold text-xl" href="" onclick={() => selectTodo(-1)}>🫑 Paprika-ToDo</a>
    {#if selectedTodo == -1}
        <button class="btn btn-emerald ms-2" onclick={() => addTodo()}>+</button>
    <!-- {:else}
        <button class="btn btn-border-rose ms-2" onclick={() => selectTodo(-1)}>Back</button> -->
    {/if}
</header>

{#if selectedTodo > -1}
    <ToDo 
        todo={todos[selectedTodo]} 
        addTodoElement={addTodoElement} 
        changeElementContent={changeElementContent} 
        changeToDoTitle={changeToDoTitle} 
        updateCookie={updateCookie} 
        selectTodo={selectTodo} 
        changeElementPosition={changeElementPosition}
        deleteElement={deleteElement}
        clearElements={clearElements}
        shuffleElements={shuffleElements}/>
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
        @apply bg-emerald-500 text-white;
    }
    .btn-emerald:hover {
        @apply bg-emerald-700;
    }

</style>



