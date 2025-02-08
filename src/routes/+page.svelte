<script>
	import Title from '$lib/Title.svelte';
</script>

<div class="home-screen-section top-section"></div>
<div class="home-screen-section mid-section">
	<Title text="saltyfoxes" main />
</div>

<style>
	.home-screen-section {
		height: 40vh;
	}

	.top-section {
		background-color: var(--color-primary);
	}

	.mid-section {
		display: flex;
		align-items: end;
		justify-content: center;
		background-color: var(--color-secondary);
	}
</style>
