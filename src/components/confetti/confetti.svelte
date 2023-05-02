<script lang="ts">
    import {onMount } from 'svelte';

    export let characters = ["*"];
    export let number = 100;
    export let speed = 0.7;
    export let wiggle = 1;
    export let wiggleSpeed = 0.05;

    let confetti = new Array(number).fill('*').map((_,i)=>{
        return {
            char: characters[ i % characters.length ],
            x: Math.random() * 100,
            y: -30 - Math.random () * 100,
            r: Math.random() * 1,
            wiggle: Math.random() * 10
        }
    }).sort((a,b)=>a.r - b.r);

    onMount(()=>{
        let frame : number;
        function loop(){
            frame = requestAnimationFrame(loop);

            confetti = confetti.map(char=>{
                char.y += speed * char.r;
                char.x += Math.sin(char.wiggle) * wiggleSpeed;
                char.wiggle+=wiggle * wiggleSpeed;
                if(char.y > 150) char.y = -50;
                return char;
            });
        }
        loop();

        return()=>cancelAnimationFrame(frame);
    });

</script>

{#each confetti as c}
    <span style="left: {c.x}%;
                top:{c.y}%;
                transform:scale({c.r})">
        {c.char}
    </span>
{/each}

<style>
    :global(body){
        overflow:hidden;
    }

    span {
        position:absolute;
        font-size:5vw;
        user-select:none;
    }
</style>