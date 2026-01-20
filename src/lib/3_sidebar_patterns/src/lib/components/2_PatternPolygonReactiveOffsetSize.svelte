<script>
	import Slider from '$lib/ui/Slider.svelte';

	let squareSize = $state(50);
	const squareCount = 1000 / squareSize;
	let squareOffset = $state(0);
</script>

<div class="svg-container">
	<svg viewBox="0 0 1000 1000" class="svg-canvas">
		{#each Array(squareCount) as _, yi}
			{#each Array(squareCount) as _, xi}
				<polygon
					transform="translate({xi * squareSize + xi * squareOffset} {yi * squareSize +
						yi * squareOffset})"
					points="0 0, {squareSize} 0, 0 {squareSize}"
					fill="hotpink"
				/>
				<polygon
					transform="translate({xi * squareSize + xi * squareOffset} {yi * squareSize +
						yi * squareOffset})"
					points="{squareSize} 0, {squareSize} {squareSize}, 0 {squareSize}"
					fill="khaki"
				/>
			{/each}
		{/each}
	</svg>
</div>

<div class="sidebar-right">
	<Slider min={0} max={50} bind:value={squareOffset} label="Square Offset" />

  <hr/>


	<Slider min={50} max={250} bind:value={squareSize} label="Square Size" />
</div>
