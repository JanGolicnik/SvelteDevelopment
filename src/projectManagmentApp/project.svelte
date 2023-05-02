<script>
  import CommentSection from "../commentSection.svelte";

   export let name = "unnamed project";
   export let todos = [];
</script>

<div class="container">
    <h1>{name}</h1>
    <CommentSection todos={todos}/>
</div>

<style>
    .container {
        border:1px solid black;
        width:fit-content;
        margin:20px;
        padding:1em;
    }
</style>