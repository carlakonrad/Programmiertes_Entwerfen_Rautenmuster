<script>
	import Slider from '$lib/ui/Slider.svelte';
	import { appState } from '$lib/appState.svelte.js';


	let squareSize = $state(50);
	let squareDistance = $state(0);

	let combinedOffset = $derived(squareDistance + squareSize);

	let squareCountX = $derived(Math.round(1000 / squareSize) + 1);
	let squareCountY = $derived(Math.round(1000 / squareSize) + 1);

	let offset = $state(0);

	// Maximale Breite für Rauten, damit sie sich nicht überlappen
	let maxWidth = $derived(squareSize / 2);

	// Begrenze appState.offset auf maximal 25 für dieses Pattern
	let constrainedOffset = $derived(Math.min(appState.offset, 25));

	// Lokale Variable für den Slider, die auf 25 begrenzt ist
	let localOffset = $state(0);
	
	// Synchronisiere localOffset mit appState.offset (begrenzt auf 25)
	$effect(() => {
		localOffset = Math.min(appState.offset, 25);
	});
	
	// Wenn localOffset sich ändert, aktualisiere appState.offset
	$effect(() => {
		appState.offset = localOffset;
	});

	// Farbton für Quadrate (Rauten sind komplementär)
	let hueRect = $state(270); // Startwert für Lila
	
	// Komplementärfarbe für Rauten (180° versetzt)
	let hueRhombus = $derived((hueRect + 180) % 360);

	// Kräftige, einheitliche Farben - alle Quadrate gleich, alle Rauten gleich
	const satRect = 100;
	const lightRect = 50;

	const satRhombus = 100;
	const lightRhombus = 40;

	function getRectColor(xi, yi) {
		return rgbToHex(...hslToRgb(hueRect, satRect, lightRect));
	}

	function getRhombusColor(xi, yi, variant = 1) {
		return rgbToHex(...hslToRgb(hueRhombus, satRhombus, lightRhombus));
	}

	// Konvertiert HSL zu RGB
	function hslToRgb(h, s, l) {
		s /= 100;
		l /= 100;
		const k = (n) => (n + h / 30) % 12;
		const a = s * Math.min(l, 1 - l);
		const f = (n) => l - a * Math.max(-1, Math.min(k(n) - 3, Math.min(9 - k(n), 1)));
		return [Math.round(255 * f(0)), Math.round(255 * f(8)), Math.round(255 * f(4))];
	}

	// Konvertiert RGB-Array zu Hex-String
	function rgbToHex(r, g, b) {
		return '#' + [r, g, b].map((x) => x.toString(16).padStart(2, '0')).join('');
	}
</script>

<div class="svg-container">
	<svg viewBox="-500 -500 1000 1000" class="svg-canvas">
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
					points="0,-{maxWidth} {constrainedOffset},0 0,{maxWidth} {-constrainedOffset},0"
					fill={getRhombusColor(xi, yi, 1)}
					stroke="none"
				/>
				<polygon
					transform="translate({(xi - squareCountX / 2) * combinedOffset + squareSize / 2} {(yi -
						squareCountY / 2) *
						combinedOffset}) rotate(90)"
					points="0,-{maxWidth} {constrainedOffset},0 0,{maxWidth} {-constrainedOffset},0"
					fill={getRhombusColor(xi, yi, 2)}
					stroke="none"
				/>
			{/each}
		{/each}
	</svg>
</div>

<div class="sidebar-right">
	<Slider bind:value={localOffset} min={0} max={25} step={1} label="Rautenbreite" />
	
	<div class="color-section">
		<Slider bind:value={hueRect} min={0} max={360} step={1} label="Farbton" />
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
