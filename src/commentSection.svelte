<script>

import { onMount } from "svelte";

export let todos = [];

let uid = 1;


onMount(()=>{
	todos.forEach((t)=>{
		t.id = uid++;
	})
})

function add(inputEl){
    const todo = {
        id: uid++,
        completed:false,
        text:inputEl.value
    }
    todos = [todo, ...todos];
    inputEl.value = "";
}

</script>

<div class="container">
    <div class="head">
        <input type=text placeholder="still gotta do..." on:keydown={e=>{
            if(e.key == "Enter"){
                add(e.target);
            }
        }}/>
    </div>
    <div class="todos">
        {#each todos as todo (todo.id)}
        <div class="todo {todo.completed ? "completed" : ""}">
            <span>
                <input checked={todo.completed} type=checkbox on:change={(e)=>{todo.completed = !todo.completed;}}/>{todo.text}
            </span>
        </div>
        {:else}
        <p>nothing to do :(</p>
        {/each}
    </div>
</div>

<style>
    .head input{
        width:100%
    }
    .todos{
        display:flex;
        flex-direction: column;
    }
    .todo {
        padding-top:10px;
        color:red;
        opacity:100%;
        transition: opacity 200ms ;
    }
    .completed{
        opacity:50%;
        color:black;
    }
</style>