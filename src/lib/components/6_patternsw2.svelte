<script>
    import chroma from 'chroma-js';
    import Slider from '$lib/ui/Slider.svelte';
    
    const squareCount = 20;
    const squareSize = 1000 / squareCount;
    
    let offset = $state(0);
    let colorStage = $state(0); // 0-3: schwarz/weiß, grau, pastell, kräftig
    
    // Basierend auf der Farbstufe die Farbparameter berechnen
    let saturation = $derived.by(() => {
        switch(colorStage) {
            case 0: return 0; // Schwarz/Weiß - keine Sättigung
            case 1: return 0; // Graustufen - keine Sättigung
            case 2: return 0.15; // Pastell - leichte Sättigung
            case 3: return 0.25; // Kräftig - hohe Sättigung
            default: return 0;
        }
    });
    
    // Helligkeitswerte für die verschiedenen Stadien
    let lightness1 = $derived.by(() => {
        switch(colorStage) {
            case 0: return 0; // Schwarz
            case 1: return 0.25; // Dunkelgrau
            case 2: return 0.55; // Pastell dunkel
            case 3: return 0.45; // Kräftig dunkel
            default: return 0;
        }
    });
    
    let lightness2 = $derived.by(() => {
        switch(colorStage) {
            case 0: return 1; // Weiß
            case 1: return 0.75; // Hellgrau
            case 2: return 0.85; // Pastell hell
            case 3: return 0.7; // Kräftig hell
            default: return 1;
        }
    });
    
    let lightness3 = $derived.by(() => {
        switch(colorStage) {
            case 0: return 1; // Weiß
            case 1: return 0.5; // Mittelgrau
            case 2: return 0.75; // Pastell mittel-hell
            case 3: return 0.6; // Kräftig mittel-hell
            default: return 1;
        }
    });
    
    let lightness4 = $derived.by(() => {
        switch(colorStage) {
            case 0: return 0; // Schwarz
            case 1: return 0.4; // Grau
            case 2: return 0.65; // Pastell mittel
            case 3: return 0.5; // Kräftig mittel
            default: return 0;
        }
    });
    
    // Vier komplementäre Farben - für Stufe 3 mit spezifischen Hex-Farben
    let color1 = $derived.by(() => {
        if (colorStage === 3) {
            return chroma('#FA5D00'); // Orange für Raute 1
        } else if (colorStage === 2) {
            return chroma('#FA5D00').set('hsl.l', 0.7).desaturate(1); // Pastell Orange
        }
        return chroma.oklch(lightness1, saturation, 180);
    });
    
    let color2 = $derived.by(() => {
        if (colorStage === 3) {
            return chroma('#00FAF9'); // Cyan für Raute 2
        } else if (colorStage === 2) {
            return chroma('#00FAF9').set('hsl.l', 0.75).desaturate(1); // Pastell Cyan
        }
        return chroma.oklch(lightness2, saturation, 270);
    });
    
    let color3 = $derived.by(() => {
        if (colorStage === 3) {
            return chroma('#A200FA'); // Lila für Rechteck 1
        } else if (colorStage === 2) {
            return chroma('#A200FA').set('hsl.l', 0.7).desaturate(1); // Pastell Lila
        }
        return chroma.oklch(lightness3, saturation, 0);
    });
    
    let color4 = $derived.by(() => {
        if (colorStage === 3) {
            return chroma('#C7FA00'); // Gelbgrün für Rechteck 2
        } else if (colorStage === 2) {
            return chroma('#C7FA00').set('hsl.l', 0.75).desaturate(1); // Pastell Gelbgrün
        }
        return chroma.oklch(lightness4, saturation, 90);
    });
        
    function getRectColor(i, j) {
        if ((i + j) % 2 === 0) {
          return color3;
        } else {
          return color4;
        }
    }
    
    // Labels für die Stufen
    const stageLabels = ['Schwarz/Weiß', 'Graustufen', 'Pastell', 'Kräftig'];
    
    </script>
    
    <div id="control">
        <Slider bind:value={offset} min={1} max={30} step={1} label="Offset" />
        <Slider bind:value={colorStage} min={0} max={3} step={1} label="Farbe: {stageLabels[colorStage]}" />
    </div>
    
    <div class="svg-container">
        <svg viewBox="-500 -500 1000 1000" class="svg-canvas">
            
            {#each Array(19) as _, i}
                {#each Array(19) as _, j}
                    <rect transform="translate({20 + (i-9) * 60} {20 + (j-9) * 60})" width="60" height="60" fill={getRectColor(i, j)}/>
                    <polygon transform="translate({20 + (i-9) * 60} {50 + (j-9) * 60})" points="0,-30 {offset},0 0,30 {-offset},0" fill={color1} stroke="none"/>
                    <polygon transform="translate({50 + (i-9) * 60} {20 + (j-9) * 60}) rotate(90)" points="0,-30 {offset},0 0,30 {-offset},0" fill={color2} stroke="none"/>
                    
                {/each}
            {/each}
    
        </svg>
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