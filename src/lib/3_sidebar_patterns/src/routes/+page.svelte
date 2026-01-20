<script>
	import Header from '$lib/components/Header.svelte';

  import { slide } from 'svelte/transition';
	import Pattern1 from '$lib/components/1_PatternPolygonReactive.svelte';
	import Pattern2 from '$lib/components/2_PatternPolygonReactiveOffsetSize.svelte';

	let patterns = [
		{
			name: 'Squares in the Grid (offset)',
			component: Pattern1,
			description:
				'A grid of squares divided into two triangles that can get an offset.'
		},
		{
			name: 'Squares in the Grid (offset and size)',
			component: Pattern2,
			description:
				'A grid of squares divided into two triangles that can get an offset and size.'
		},
	];

	let selectedPattern = $state(0);
	let SelectedPattern = $derived(patterns[selectedPattern].component);
</script>

<div class="app-container">
	<Header />
	<main class="app-main">
		<div class="sidebar-left">
			{#each patterns as pattern, index}
				<button
					class="sidebar-left-item"
					class:selected={selectedPattern === index}
					onclick={() => (selectedPattern = index)}
					>{pattern.name}
					{#if selectedPattern === index}
						<div transition:slide class="sidebar-left-description">{pattern.description}</div>
					{/if}
				</button>
			{/each}
		</div>

		<SelectedPattern />
	</main>
</div>