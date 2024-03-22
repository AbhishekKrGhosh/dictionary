<script>
import Loader from "./Loader.svelte";
import WordData  from "./WordData.svelte";

let word = "";
let loading = false;
let wordData = null;
async function searchWord(){
	if(word.length === 0){
		return;
	}
	loading = true;
	wordData = null;
	try {
		let res = await fetch("https://api.dictionaryapi.dev/api/v2/entries/en/"+word);
		let data = await res.json();
		if(Array.isArray(data)){
			wordData = data[0];
			
		} else {
			wordData = "No result";
		}
	}
	catch(err) {
		loading = false;
	}
}
</script>
<style>
	.header{
		width: 100%;
		padding: 20px;
		background: #111;
		color: #f5f5f5;
		font-size: 20px;
		text-align: center;
	}
	.center{
		margin: 40px auto;
		width: 95%;
		max-width: 550px;
		background: #fff;
	}
	.form{
		background: #fff;
		padding: 20px;
		border: 1px solid #f5f5f5;
		box-shadow: 0px 0px 5px 2px rgba(0,0,0,0.05);
		margin: 20px 0px;
	}
	.form .form-group{
		margin: 10px 0px;
	}
	.form .form-group label{
		display: block;
		font-size: 16px;
		margin-bottom: 8px;
	}
	.form .form-group input{
		width: 100%;
		padding: 10px;
		font-size: 16px;
		border: 1px solid #bbb;
	}
	.form .form-group button{
		padding: 10px 20px;
		background: #111;
		color: #f5f5f5;
		font-size: 15px;
		border: none;
		outline: none;
		cursor: pointer;
	}
	.result{
		background: #fff;
		border: 1px solid #f5f5f5;
		box-shadow: 0px 0px 5px 2px rgba(0,0,0,0.05);
	}
	.result .padding{
		padding: 20px;
	}
</style>
<main>
<div class="header">Dictionary App</div>
<div class="center">
	<div class="form">
		<div class="form-group">
			<label>Search Word</label>
			<input type="text" placeholder="Type word here" bind:value={word}/>
		</div>
		<div class="form-group">
			<button on:click={searchWord}>Search</button>
		</div>
	</div>
	{#if loading === true || wordData !== null}
	<div class="result">
	{#if wordData !==  null && typeof wordData !== "string"}
	<WordData wordData={wordData}/>
	{:else if wordData ===  null && loading === true}
	<Loader/>
	{:else}
	<p class="padding">No result found</p>
	{/if}
	</div>
	{/if}
</div>
</main>

