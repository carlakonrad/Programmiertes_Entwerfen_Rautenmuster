<script>
    import chroma from 'chroma-js';
    import Slider from '$lib/ui/Slider.svelte';
    import { appState } from '$lib/appState.svelte.js';

    
    const squareCount = 20;
    const squareSize = 1000 / squareCount;
    
    let offset = $state(0);
    let colorIntensity = $state(0); // 0-1: smooth von schwarz/weiß zu kräftigen Farben
    
    // Kontinuierliche Sättigung mit langsamerer Kurve - mehr Grauwerte am Anfang
    // Quadratische Funktion für sanfteren Übergang
    let saturation = $derived(Math.pow(colorIntensity, 2) * 0.25);
    
    // Helligkeitswerte interpolieren smooth zwischen den Extremen
    let lightness1 = $derived(0 + colorIntensity * 0.45); // von 0 (schwarz) zu 0.45
    let lightness2 = $derived(1 - colorIntensity * 0.3); // von 1 (weiß) zu 0.7
    let lightness3 = $derived(1 - colorIntensity * 0.4); // von 1 (weiß) zu 0.6
    let lightness4 = $derived(0 + colorIntensity * 0.5); // von 0 (schwarz) zu 0.5
    
    // Vier komplementäre Farben - smooth Übergang zu den Zielfarben
    let color1 = $derived.by(() => {
        const baseColor = chroma('#FA5D00'); // Orange für Raute 1
        return chroma.oklch(lightness1, saturation, 180).mix(baseColor, colorIntensity, 'oklch');
    });
    
    let color2 = $derived.by(() => {
        const baseColor = chroma('#00FAF9'); // Cyan für Raute 2
        return chroma.oklch(lightness2, saturation, 270).mix(baseColor, colorIntensity, 'oklch');
    });
    
    let color3 = $derived.by(() => {
        const baseColor = chroma('#A200FA'); // Lila für Rechteck 1
        return chroma.oklch(lightness3, saturation, 0).mix(baseColor, colorIntensity, 'oklch');
    });
    
    let color4 = $derived.by(() => {
        const baseColor = chroma('#C7FA00'); // Gelbgrün für Rechteck 2
        return chroma.oklch(lightness4, saturation, 90).mix(baseColor, colorIntensity, 'oklch');
    });
        
    function getRectColor(i, j) {
        if ((i + j) % 2 === 0) {
          return color3;
        } else {
          return color4;
        }
    }
    
</script>



<div class="svg-container">
    <svg viewBox="-500 -500 1000 1000" class="svg-canvas">
        
        {#each Array(19) as _, i}
            {#each Array(19) as _, j}
                <rect transform="translate({20 + (i-9) * 60} {20 + (j-9) * 60})" width="60" height="60" fill={getRectColor(i, j)}/>
                <polygon transform="translate({20 + (i-9) * 60} {50 + (j-9) * 60})" points="0,-30 {appState.offset},0 0,30 {-appState.offset},0" fill={color1} stroke="none"/>
                <polygon transform="translate({50 + (i-9) * 60} {20 + (j-9) * 60}) rotate(90)" points="0,-30 {appState.offset},0 0,30 {-appState.offset},0" fill={color2} stroke="none"/>
                
            {/each}
        {/each}

    </svg>
</div>
    
<div class="sidebar-right">
    <Slider bind:value={appState.offset} min={0} max={30} step={1} label="Rautenbreite" />
    <Slider bind:value={colorIntensity} min={0} max={1} step={0.01} label="Sättigung" />
</div>


    <style>
        #control {
          display: flex;
          flex-direction: column;
          width: 300px;
          gap: 10px;
          justify-content: left;
          align-items: flex-start;
        }
    </style>