<!--
The header goes through two animations on the main page:
1. As the user scrolls, the top-section and mid-section divs shrink, the text shrinks, and the text's margin-bottom shrinks. This is controlled by JS.
2. Once the scroll has been completed, the user is prevented from scrolling back up and the title text slides to the left. This is controlled by CSS.

On other pages, the animation is skipped.
-->

<script lang="ts">
	let {
		text,
		isHome = false,
		pastScrollLock = $bindable(!isHome)
	}: { text: string; isHome?: boolean; pastScrollLock?: boolean } = $props();

	let windowScroll: number | undefined = $state();
	let windowHeight: number | undefined = $state();
	let scrollInterval = $derived.by(() => {
		if (pastScrollLock) {
			return 1;
		}

		if (windowScroll === undefined || windowHeight === undefined) {
			return 0;
		}

		return Math.min(windowScroll / windowHeight, 1);
	});

	let [blockStyle, titleStyle] = $derived([
		`height: ${interpolate(4, 36, 1 - scrollInterval)}vh`,
		`font-size: ${interpolate(3, 5, 1 - scrollInterval)}vh;
        margin-bottom: ${interpolate(0, 5, 1 - scrollInterval)}vh;`
	]);

	$effect(() => {
		if (!pastScrollLock && scrollInterval === 1) {
			pastScrollLock = true;
		}
	});

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
		{#if isHome && pastScrollLock}
			<h1 style={titleStyle} class="slide-text-left"><span>{text}</span></h1>
		{:else if pastScrollLock}
			<h1 style={titleStyle} class="text-left">{text}</h1>
		{:else}
			<h1 style={titleStyle}><span>{text}</span></h1>
		{/if}
	</div>
</div>

{#if !pastScrollLock}
	<div class="initial-screen-container"></div>
{/if}

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

	.initial-screen-container {
		height: 200vh;
	}

	.slide-text-left {
		animation: slide-title-left forwards 2s;
	}

	.slide-text-left span {
		animation: undo-text-translate forwards 2s;
	}

	.text-left {
		margin-left: 2em !important;
		translate: 0 !important;
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
