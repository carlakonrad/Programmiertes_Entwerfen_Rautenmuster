<script>
    import chroma from 'chroma-js';
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

	// Gemeinsamer Farbton für alle Elemente
	let hue = $state(270); // Startwert für Lila

	// Globale Sättigungs- und Helligkeitsanpassung
	let saturationOffset = $state(0);
	let lightnessOffset = $state(0);

	// Feste Sättigung und Helligkeit für jedes Element
	const satRect1 = 0.10;
	const lightRect1 = 0.10;
	const satRect2 = 0.15;
	const lightRect2 = 0.15;

	const satRhombus1 = 0.20;
	const lightRhombus1 = 0.20;
	const satRhombus2 = 0.25;
	const lightRhombus2 = 0.25;

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

	// Derived colors mit Sättigungs- und Helligkeits-Offsets
	let colorRect1 = $derived(
        chroma.hsl(hue, (satRect1 + saturationOffset) % 1, (lightRect1 + lightnessOffset) % 1).hex()
		// rgbToHex(...hslToRgb(hue, Math.max(0, Math.min(100, satRect1 + saturationOffset)), Math.max(0, Math.min(100, lightRect1 + lightnessOffset))))
	);
	let colorRect2 = $derived(
		// rgbToHex(...hslToRgb(hue, Math.max(0, Math.min(100, satRect2 + saturationOffset)), Math.max(0, Math.min(100, lightRect2 + lightnessOffset))))
        chroma.hsl(hue, (satRect2 + saturationOffset) % 1, (lightRect2 + lightnessOffset) % 1).hex()
	);
	let colorRhombus1 = $derived(
		// rgbToHex(...hslToRgb(hue, Math.max(0, Math.min(100, satRhombus1 + saturationOffset)), Math.max(0, Math.min(100, lightRhombus1 + lightnessOffset))))
        chroma.hsl(hue, (satRhombus1 + saturationOffset) % 1, (lightRhombus1 + lightnessOffset) % 1).hex()
	);
	let colorRhombus2 = $derived(
		// rgbToHex(...hslToRgb(hue, Math.max(0, Math.min(100, satRhombus2 + saturationOffset)), Math.max(0, Math.min(100, lightRhombus2 + lightnessOffset))))
        chroma.hsl(hue, (satRhombus2 + saturationOffset) % 1, (lightRhombus2 + lightnessOffset) % 1).hex()
	);

	function getRectColor(xi, yi) {
		if ((xi + yi) % 2 === 0) {
			return colorRect1;
		} else {
			return colorRect2;
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
				points="0,-{maxWidth} {constrainedOffset},0 0,{maxWidth} {-constrainedOffset},0"
				fill={colorRhombus1}
				stroke="none"
			/>
			<polygon
				transform="translate({(xi - squareCountX / 2) * combinedOffset + squareSize / 2} {(yi -
					squareCountY / 2) *
					combinedOffset}) rotate(90)"
				points="0,-{maxWidth} {constrainedOffset},0 0,{maxWidth} {-constrainedOffset},0"
					fill={colorRhombus2}
					stroke="none"
				/>
			{/each}
		{/each}
	</svg>
</div>

<div class="sidebar-right">
	<Slider bind:value={localOffset} min={0} max={25} step={1} label="Rautenbreite" />
	
	<div class="color-section">
		<h3 style="margin: 20px 0 10px 0; font-size: 1rem; font-weight: 500;">Farbe</h3>
		<Slider bind:value={hue} min={0} max={360} step={1} label="Farbton" />
		<Slider bind:value={saturationOffset} min={0} max={0.74} step={0.01} label="Sättigung" />
		<Slider bind:value={lightnessOffset} min={0} max={0.74} step={0.01} label="Helligkeit" />
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
