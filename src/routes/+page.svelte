<script>
  import Nekidruzgaapp from "./nekidruzgaapp.svelte";
	import Projectmanagmentapp from "./projectmanagmentapp.svelte";
	const apps = [{id:1, component:Projectmanagmentapp},
					{id:2, component:Nekidruzgaapp}];
	let currentApp = 1;
</script>

<select on:change={e=>{currentApp = e.target.value}}>
	<option value="1">Project managment app</option>
	<option value="2">app2</option>
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
