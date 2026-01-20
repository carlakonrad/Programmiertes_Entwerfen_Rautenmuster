<script>
	import ColorPickerHSV from '$lib/ui/ColorPicker/ColorPickerHSV.svelte';
	import Slider from '$lib/ui/Slider.svelte';

	let squareSize = $state(50);
	let squareDistance = $state(0);

	let combinedOffset = $derived(squareDistance + squareSize);

	let squareCountX = $derived(Math.round(1000 / squareSize) + 1);
	let squareCountY = $derived(Math.round(1000 / squareSize) + 1);

	let offset = $state(0);

	// Maximale Breite für Rauten, damit sie sich nicht überlappen
	let maxWidth = $derived(squareSize / 2);

	// Farben für die verschiedenen Elemente
	let color1 = $state('#8b5cf6'); // Raute vertikal
	let color2 = $state('#a78bfa'); // Raute horizontal
	let color3 = $state('#ddd6fe'); // Rechteck 1
	let color4 = $state('#ede9fe'); // Rechteck 2

	// Aktive Farbe für ColorPicker
	let activeColor = $state(1);

	function getRectColor(xi, yi) {
		if ((xi + yi) % 2 === 0) {
			return color3;
		} else {
			return color4;
		}
	}
</script>

<div class="svg-container">
	<svg viewBox="-500 -500 1000 1000" class="svg-canvas">
		hallo
		{#each Array(squareCountX) as _, xi}
			{#each Array(squareCountY) as _, yi}
				<rect
					x={(xi - squareCountX / 2) * combinedOffset}
					y={(yi - squareCountY / 2) * combinedOffset}
					width={squareSize}
					height={squareSize}
					fill={getRectColor(xi, yi)}
					data-x={xi}
					data-y={yi}
				/>
				<polygon
					transform="translate({(xi - squareCountX / 2) * combinedOffset} {(yi - squareCountY / 2) *
						combinedOffset +
						squareSize / 2})"
					points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0"
					fill={color1}
					stroke="none"
				/>
				<polygon
					transform="translate({(xi - squareCountX / 2) * combinedOffset + squareSize / 2} {(yi -
						squareCountY / 2) *
						combinedOffset}) rotate(90)"
					points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0"
					fill={color2}
					stroke="none"
				/>
			{/each}
		{/each}
	</svg>
</div>

<div class="sidebar-right">
	<Slider bind:value={squareSize} min={10} max={100} step={1} label="Quadratgröße" />
	<Slider bind:value={squareDistance} min={0} max={50} step={1} label="Abstand" />
	<Slider bind:value={offset} min={0} max={25} step={1} label="Offset" />

	<div class="color-selector">
		<div class="color-squares">
			<button
				class="color-square"
				class:active={activeColor === 1}
				style="background-color: {color1}"
				onclick={() => (activeColor = 1)}
				title="Raute vertikal"
			></button>
			<button
				class="color-square"
				class:active={activeColor === 2}
				style="background-color: {color2}"
				onclick={() => (activeColor = 2)}
				title="Raute horizontal"
			></button>
			<button
				class="color-square"
				class:active={activeColor === 3}
				style="background-color: {color3}"
				onclick={() => (activeColor = 3)}
				title="Rechteck 1"
			></button>
			<button
				class="color-square"
				class:active={activeColor === 4}
				style="background-color: {color4}"
				onclick={() => (activeColor = 4)}
				title="Rechteck 2"
			></button>
		</div>

		<div class="color-picker-container">
			{#if activeColor === 1}
				<ColorPickerHSV bind:color={color1} width={250} />
			{:else if activeColor === 2}
				<ColorPickerHSV bind:color={color2} width={250} />
			{:else if activeColor === 3}
				<ColorPickerHSV bind:color={color3} width={250} />
			{:else if activeColor === 4}
				<ColorPickerHSV bind:color={color4} width={250} />
			{/if}
		</div>
	</div>
</div>

<style>
	.color-selector {
		display: flex;
		flex-direction: column;
		gap: 15px;
		width: 100%;
	}

	.color-squares {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 8px;
		width: 100%;
	}

	.color-square {
		aspect-ratio: 1;
		border: 3px solid transparent;
		border-radius: 8px;
		cursor: pointer;
		transition: all 0.2s ease;
		padding: 0;
	}

	.color-square:hover {
		transform: scale(1.1);
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
	}

	.color-square.active {
		border-color: #fff;
		box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.5);
		transform: scale(1.05);
	}

	.color-picker-container {
		padding: 15px;
		background-color: rgba(0, 0, 0, 0.1);
		border-radius: 8px;
	}
</style>
