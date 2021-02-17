<script>
	import Force from "../components/Force.svelte";
	import Bar from "../components/Bar.svelte";

	let active = "force";
</script>

<style>
</style>

<svelte:head>
	<title>Force Directed Graph</title>
</svelte:head>
<button on:click={() => (active = "force")}>Force</button>
<button on:click={() => (active = "bar")}>Bar</button>
<div>
	{#key active}
		{#if active === "force"}
			<Force />
		{:else if active === "bar"}
			<Bar />
		{/if}
	{/key}
</div>
