<script lang="ts">
	import Header from '$lib/Header.svelte';

	let pastScrollLock = $state(false);
</script>

<Header text="saltyfoxes" bind:pastScrollLock isHome />

{#if pastScrollLock}
	<main>Some page content!</main>
{/if}
