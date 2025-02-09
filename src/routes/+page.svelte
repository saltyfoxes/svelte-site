<script lang="ts">
	import Header from '$lib/Header.svelte';

	let pastScrollLock = $state(false);
</script>

<Header text="saltyfoxes" bind:pastScrollLock isHome />

{#if pastScrollLock}
	<main>
		<div class="bio-section">
			<span class="about">About the artist, lorem ipsum blah blah blah blah</span>
			<enhanced:img class="fox-icon" src="/static/icon.png" alt="An icon of a Fox" />
		</div>

		<div class="links-section">
			<figure>
				<figcaption>portfolio</figcaption>
				<enhanced:img class="figimage" src="/static/home-placeholder.png" alt="placeholder" />
			</figure>

			<figure>
				<figcaption>commissions</figcaption>
				<enhanced:img class="figimage" src="/static/home-placeholder.png" alt="placeholder" />
			</figure>

			<figure>
				<figcaption>find me</figcaption>
				<enhanced:img class="figimage" src="/static/home-placeholder.png" alt="placeholder" />
			</figure>
		</div>
	</main>
{/if}

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-left: 10vw;
		margin-right: 10vw;

		gap: 2em;
	}

	.bio-section {
		margin-top: 10vh;

		display: flex;
		gap: 2em;
		width: 100%;
		justify-content: space-between;
	}

	.bio-section .fox-icon {
		height: 25vmin;
		width: 25vmin;
	}

	.about {
		font-size: 2vmin;
	}

	.links-section {
		display: flex;

		justify-content: center;
	}

	@media screen and (max-width: 750px) {
		.links-section {
			flex-direction: column;
		}
	}

	figure {
		display: flex;

		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	figure .figimage {
		height: 25vmin;
		width: 25vmin;

		aspect-ratio: auto;
	}

	figcaption {
		display: inline;
		text-align: center;
		font-family: 'Katibeh';
		font-size: 3em;
	}
</style>
