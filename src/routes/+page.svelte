<script>
	import chroma from 'chroma-js';
	import Header from '$lib/components/Header.svelte';
	import { slide } from 'svelte/transition';
	// import Pattern from '$lib/components/1_PatternRect.svelte';	
	// import Pattern from '$lib/components/2_patternRectStruct.svelte';
	// import Pattern from '$lib/components/3_pattern.svelte';
	// import Pattern from '$lib/components/4_pattern.svelte';
	// import Pattern from '$lib/components/4_1_pattern.svelte';
	// import Pattern from '$lib/components/5_pattern.svelte';
	import Pattern1 from '$lib/components/6_patternsw.svelte';
	// import Pattern from '$lib/components/6_patternsw2.svelte';
	// import Pattern from '$lib/components/6_patternsw3.svelte';
	// import Pattern from '$lib/components/6_patternsw3_1.svelte';
	import Pattern2 from '$lib/components/6_patternsw3_2.svelte';


import UiPattern from '$lib/components/1_ui_pattern.svelte';


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

let selectedPattern = $state(1);
let SelectedPattern = $derived(patterns[selectedPattern].component);
</script>

<!-- <div class="app-container">
	<Header />
	<main class="app-main">

		<Pattern />
	</main>
</div> -->

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