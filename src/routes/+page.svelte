<script lang="ts">
  	import CuteHeaderComponent from "./cuteHeaderComponent.svelte";
	import Projectmanagmentapp from "./projectmanagmentapp.svelte";
	import Confetti from "./confetti.svelte";

	const apps = [{id:1, name:"Project managment app (read like half the tutorial)", component:Projectmanagmentapp},
					{id:2, name:"A cute header component (svg fun)", component:CuteHeaderComponent},
					{id:3, name:"Confetti", component:Confetti}];

	let currentApp = 1;

</script>

<select value={currentApp} on:change={e=>{currentApp = e.target?.value}}>
	{#each apps as app}
		<option value={app.id}>{app.name}</option>
	{/each}
</select>

<div class="line"></div>

{#each apps as app}
	{#if app.id == currentApp}
		<svelte:component this={app.component}></svelte:component>
	{/if}
{/each}

<style>
	.line{
		width:100%;
		height:5px;
		background-color: black;
		margin-top:10px;
		margin-bottom:10px;
	}
</style>
