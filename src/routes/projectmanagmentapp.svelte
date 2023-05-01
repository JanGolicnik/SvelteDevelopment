<script language="typescript">

	import Project from "../components/projectManagmentApp/project.svelte";

	let uid = 1;

	let projects = [
		{id:uid++, name:"Conquer the world", todos:[{text:"kill everyone", completed:false}]},
		{id:uid++, name:"Make a project managment app", todos:[{text:"make everything", completed:true}]},
		{id:uid++, name:"Make terraria from scratch", todos:[{text:"make everything", completed:true}]},
	];

	function add(input){
		const project = {
			id:uid++,
			name: input.value,
			todos:[]
		};

		projects = [project, ...projects];
		input.value="";
	}
</script>

<input type=text name=project placeholder="Whats my next project hmm" on:keydown={e=>{if(e.key==="Enter")add(e.target);}}/>

<div class="projects">
	{#each projects as project (project.id)}
		<Project name={project.name} todos={project.todos}/>
	{/each}
</div>

<style>

input{
	width:100%;
	height:50px;
	padding-left: 15px;
}

.projects{
	display:flex;
	flex-wrap: wrap;
}

</style>
