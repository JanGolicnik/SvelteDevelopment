<script>

import { onMount } from "svelte";
import {flip} from 'svelte/animate';
import {quintOut} from 'svelte/easing';
  import { flush_render_callbacks } from "svelte/internal";
import {crossfade} from 'svelte/transition';

const [send, receive] = crossfade({
    duration: d=> Math.sqrt(d*1600),
});


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

        {#each todos.filter(e=>!e.completed) as todo (todo.id)}
            <div in:receive="{{key: todo.id}}" out:send="{{key: todo.id}}" animate:flip class="todo {todo.completed ? "completed" : ""}">
                <input checked={todo.completed} type=checkbox on:change={(e)=>{todo.completed = !todo.completed;}}/>{todo.text}
            </div>
        {/each}

        {#each todos.filter(e=>e.completed) as todo (todo.id)}
            <div in:receive="{{key: todo.id}}" out:send="{{key: todo.id}}" animate:flip class="todo {todo.completed ? "completed" : ""}">
                <input checked={todo.completed} type=checkbox on:change={(e)=>{todo.completed = !todo.completed;}}/>{todo.text}
            </div>
        {/each}

        {#if todos.length === 0}
            <p>nothing to do :(</p>
        {/if}

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