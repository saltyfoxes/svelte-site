<!--
The header goes through two animations on the main page:
1. As the user scrolls, the top-section and mid-section divs shrink, the text shrinks, and the text's margin-bottom shrinks. This is controlled by JS.
2. Once the scroll has been completed, the user is prevented from scrolling back up and the title text slides to the left. This is controlled by CSS.

On other pages, the animation is skipped.
-->

<script lang="ts">
	let { text, isHome = false }: { text: string; isHome?: boolean } = $props();

	let windowScroll: number | undefined = $state();
	let windowHeight: number | undefined = $state();
	let scrollInterval = $derived.by(() => {
		if (!isHome) {
			return 1;
		}

		if (windowScroll === undefined || windowHeight === undefined) {
			return 0;
		}

		return Math.min(windowScroll / windowHeight, 1);
	});

	let [blockStyle, titleStyle] = $derived([
		`
            height: ${interpolate(4, 36, 1 - scrollInterval)}vh
        `,
		`
            font-size: ${interpolate(2, 5, 1 - scrollInterval)}em;
            margin-bottom: ${interpolate(0, 5, 1 - scrollInterval)}vh;
        `
	]);

	function interpolate(from: number, to: number, interval: number) {
		return to * interval + from;
	}
</script>

<svelte:head>
	<title>{text}</title>
</svelte:head>

<svelte:window bind:scrollY={windowScroll} bind:innerHeight={windowHeight} />

<div class="header-container">
	<div style={blockStyle} class="top-section"></div>
	<div style={blockStyle} class="mid-section">
		{#if scrollInterval === 1}
			<h1 style={titleStyle} class="animating"><span>{text}</span></h1>
		{:else}
			<h1 style={titleStyle}><span>{text}</span></h1>
		{/if}
	</div>
</div>

<div class="bottom-section"></div>

<style>
	.top-section {
		background-color: var(--color-primary);
	}

	.mid-section {
		display: flex;
		align-items: end;
		background-color: var(--color-secondary);
	}

	.mid-section h1 {
		font-family: Katibeh;
		color: var(--color-white);

		display: block;
		width: 100%;
		translate: 50%;
	}

	.mid-section h1 span {
		display: inline-block;
		translate: -50%;
	}

	.header-container {
		top: 0;
		width: 100vw;
		position: fixed;
	}

	.bottom-section {
		height: 40vh;
	}

	.animating {
		animation: slide-title-left forwards 2s;
	}

	.animating span {
		animation: undo-text-translate forwards 2s;
	}

	@keyframes slide-title-left {
		from {
			translate: 50%;
		}

		to {
			margin-left: 2em;
			translate: 0;
		}
	}

	@keyframes undo-text-translate {
		from {
			translate: -50%;
		}
		to {
			translate: 0;
		}
	}
</style>
