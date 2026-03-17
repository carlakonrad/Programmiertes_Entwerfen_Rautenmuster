<script>
	import chroma from 'chroma-js';
	import Header from '$lib/components/Header.svelte';
	import { slide } from 'svelte/transition';
	import Pattern1 from '$lib/components/pattern_rautendynamik.svelte';
	import Pattern2 from '$lib/components/pattern_rautendynamik2.svelte';
	import Pattern3 from '$lib/components/pattern_Rautendynamik_Progressive_Sättigung.svelte';
	import Pattern4 from '$lib/components/pattern_Rautendynamik_Komplementäre_Kontrastpaare.svelte';
	import Pattern5 from '$lib/components/pattern_Rautendynamik_Monochrome_Harmonie.svelte';
	import Pattern6 from '$lib/components/pattern_Rautendynamik_Invertierter_Farbverlauf.svelte';





import UiPattern from '$lib/components/1_ui_pattern.svelte';


let patterns = [
  {
    name: 'Rautendynamik',
    component: Pattern1,
    description:
      'Die mathematische Präzision wird zum flüssigen Erlebnis. Ein Spiel mit der Breite, das aus statischen Rastern eine fließende visuelle Dynamik macht.'
  },
  {
    name: 'Rautendynamik II',
    component: Pattern2,
    description:
      'Eine Abwandlung der Rautendynamik.'
  },
    {
    name: 'Rautendynamik: Progressive Sättigung',
    component: Pattern3,
    description:
      'Mit steigender Farbsättigung wandelt sich das schlichte Raster in ein komplexes Farbmosaik. Durch die chromatische Abstufung treten die Raute und das Quadrat als klare, eigenständige Bestandteile des Musters hervor.'
  },
  {
    name: 'Rautendynamik: Komplementäre Kontrastpaare',
    component: Pattern4,
    description:
      'Durch die maximale Sättigung gegenüberliegender Farben wandelt sich das Raster in ein vibrierendes Farbgefüge. Diese extreme optische Spannung lässt die Raute und das Quadrat als scharf voneinander getrennte, fast leuchtende Bestandteile des Musters hervortreten.'
  },
     {
    name: 'Rautendynamik: Monochrome Harmonie',
    component: Pattern5,
    description:
      'In dieser sanften Tonalität treten die Raute und das Quadrat als weiche, harmonische Bestandteile des Musters hervor.'
  },
    {
    name: 'Rautendynamik: Invertierter Farbverlauf',
    component: Pattern6,
    description:
      'Die Veränderung der Rautenform manipuliert direkt den Fluss der Farben innerhalb des Rasters. Der Schieberegler bewirkt einen sichtbaren Wandel in der Richtung des Farbverlaufs und schafft so ein dynamisch invertiertes Bildfeld.'
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