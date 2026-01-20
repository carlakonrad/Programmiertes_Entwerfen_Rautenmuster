<script>
    import chroma from 'chroma-js';
    import Slider from '$lib/ui/Slider.svelte';
    
    const squareSize = 50;
    const squareDistance = 0;
    
    const combinedOffset = squareDistance + squareSize;
    
    const squareCountX = 1000 / squareSize;
    const squareCountY = 1000 / squareSize;
    
    let offset = $state(0);
    
    // Maximale Breite für Rauten, damit sie sich nicht überlappen
    const maxWidth = squareSize / 2; // 25
    
    // Individuelle Steuerung für Farbe 1 (Raute 1)
    let hue1 = $state(0);
    let saturation1 = $state(0);
    let lightness1 = $state(0);
    
    // Individuelle Steuerung für Farbe 2 (Raute 2)
    let hue2 = $state(90);
    let saturation2 = $state(0);
    let lightness2 = $state(1);
    
    // Individuelle Steuerung für Farbe 3 (Rechteck 1)
    let hue3 = $state(180);
    let saturation3 = $state(0);
    let lightness3 = $state(1);
    
    // Individuelle Steuerung für Farbe 4 (Rechteck 2)
    let hue4 = $state(270);
    let saturation4 = $state(0);
    let lightness4 = $state(0);
    
    // Vier komplementäre Farben mit individuellen Einstellungen
    let color1 = $derived(chroma.oklch(lightness1, saturation1, hue1));
    let color2 = $derived(chroma.oklch(lightness2, saturation2, hue2));
    let color3 = $derived(chroma.oklch(lightness3, saturation3, hue3));
    let color4 = $derived(chroma.oklch(lightness4, saturation4, hue4));
        
    function getRectColor(xi, yi) {
        if ((xi + yi) % 2 === 0) {
          return color3;
        } else {
          return color4;
        }
    }
    
</script>

<div id="control">
    <Slider bind:value={offset} min={1} max={25} step={1} label="Offset" />
    
    <div class="color-section">
        <h3>Farbe 1 (Raute vertikal)</h3>
        <Slider bind:value={hue1} min={0} max={360} step={1} label="Farbton" />
        <Slider bind:value={saturation1} min={0} max={0.37} step={0.01} label="Sättigung" />
        <Slider bind:value={lightness1} min={0} max={1} step={0.01} label="Helligkeit" />
    </div>
    
    <div class="color-section">
        <h3>Farbe 2 (Raute horizontal)</h3>
        <Slider bind:value={hue2} min={0} max={360} step={1} label="Farbton" />
        <Slider bind:value={saturation2} min={0} max={0.37} step={0.01} label="Sättigung" />
        <Slider bind:value={lightness2} min={0} max={1} step={0.01} label="Helligkeit" />
    </div>
    
    <div class="color-section">
        <h3>Farbe 3 (Rechteck 1)</h3>
        <Slider bind:value={hue3} min={0} max={360} step={1} label="Farbton" />
        <Slider bind:value={saturation3} min={0} max={0.37} step={0.01} label="Sättigung" />
        <Slider bind:value={lightness3} min={0} max={1} step={0.01} label="Helligkeit" />
    </div>
    
    <div class="color-section">
        <h3>Farbe 4 (Rechteck 2)</h3>
        <Slider bind:value={hue4} min={0} max={360} step={1} label="Farbton" />
        <Slider bind:value={saturation4} min={0} max={0.37} step={0.01} label="Sättigung" />
        <Slider bind:value={lightness4} min={0} max={1} step={0.01} label="Helligkeit" />
    </div>
</div>

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
                    transform="translate({(xi - squareCountX / 2) * combinedOffset} {(yi - squareCountY / 2) * combinedOffset + squareSize / 2})" 
                    points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0" 
                    fill={color1} 
                    stroke="none"
                />
                <polygon 
                    transform="translate({(xi - squareCountX / 2) * combinedOffset + squareSize / 2} {(yi - squareCountY / 2) * combinedOffset}) rotate(90)" 
                    points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0" 
                    fill={color2} 
                    stroke="none"
                />
                
            {/each}
        {/each}
        
        <!-- Zusätzliche vertikale Rauten am rechten Rand -->
        {#each Array(squareCountY) as _, yi}
            <polygon 
                transform="translate({(squareCountX - squareCountX / 2) * combinedOffset} {(yi - squareCountY / 2) * combinedOffset + squareSize / 2})" 
                points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0" 
                fill={color1} 
                stroke="none"
            />
        {/each}
        
        <!-- Zusätzliche horizontale Rauten am unteren Rand -->
        {#each Array(squareCountX) as _, xi}
            <polygon 
                transform="translate({(xi - squareCountX / 2) * combinedOffset + squareSize / 2} {(squareCountY - squareCountY / 2) * combinedOffset}) rotate(90)" 
                points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0" 
                fill={color2} 
                stroke="none"
            />
        {/each}
        
        <!-- Raute in der unteren rechten Ecke (vertikal) -->
        <polygon 
            transform="translate({(squareCountX - squareCountX / 2) * combinedOffset} {(squareCountY - squareCountY / 2) * combinedOffset + squareSize / 2})" 
            points="0,-{maxWidth} {offset},0 0,{maxWidth} {-offset},0" 
            fill={color1} 
            stroke="none"
        />
    </svg>
</div>

<style>
    #control {
      display: flex;
      flex-direction: column;
      width: 300px;
      gap: 20px;
      justify-content: left;
      align-items: flex-start;
    }
    .color-section {
      width: 100%;
      border-top: 1px solid #444;
      padding-top: 10px;
    }
    .color-section h3 {
      font-size: 0.85rem;
      color: #aaa;
      margin: 0 0 10px 0;
    }
</style>
