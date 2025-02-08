<script lang="ts">
	import '@fontsource/katibeh';

	const { text, main = false }: { text: string; main?: boolean } = $props();
</script>

<svelte:head>
	<title>{text}</title>
</svelte:head>

{#if main}
	<h1>{text}</h1>
{:else}
	<h3>{text}</h3>
{/if}

<style>
	h1,
	h3 {
		font-family: Katibeh;
		color: var(--color-white);
	}

	h1 {
		font-size: 5em;
	}
</style>
