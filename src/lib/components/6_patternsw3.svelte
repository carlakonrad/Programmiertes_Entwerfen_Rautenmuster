<script>
    import chroma from 'chroma-js';
    
    const squareCount = 20;
    const squareSize = 1000 / squareCount;
    
    let offset = $state(0);
    let hue = $state(0);
    
    
    // Sättigung steigt mit hue-Wert: bei 0 = keine Farbe, bei 360 = volle Sättigung
    let saturation = $derived(hue / 360 * 0.37); // Maximale Sättigung erhöht für bunte Farben
    
    // Helligkeitswerte: bei hue=0 schwarz/weiß, bei hue>0 passende Helligkeit für Farben
    let lightness1 = $derived(hue === 0 ? 0 : 0.5);
    let lightness2 = $derived(hue === 0 ? 1 : 0.85);
    
    // Vier komplementäre Farben - bei hue=0: color1=schwarz, color2=weiß, color3=weiß, color4=schwarz
    let color1 = $derived(chroma.oklch(lightness1, saturation, hue)); // Schwarz → Farbe
    let color2 = $derived(chroma.oklch(lightness2, saturation, hue + 90)); // Weiß → Farbe
    let color3 = $derived(chroma.oklch(lightness2, saturation, hue + 180)); // Weiß → Farbe
    let color4 = $derived(chroma.oklch(lightness1, saturation, hue + 270)); // Schwarz → Farbe
        
    function getRectColor(i, j) {
        if ((i + j) % 2 === 0) {
          return color3;
        } else {
          return color4;
        }
    }
    
    </script>
    
    <div id="control">
        <input id="offset" type="range" min="1" max="30" bind:value={offset} />
        <label for="offset">{offset}</label>   
        <input id="hue" type="range" min="0" max="360" bind:value={hue} />
        <label for="hue">{hue}</label>  
        
    </div>
    
    <div class="svg-container">
        <svg viewBox="-500 -500 1000 1000" class="svg-canvas">
            
            <!-- <polygon points="20,20 40,50 20,80 0,50" fill="white" stroke="none"/>
            <polygon points="50,0 80,20 50,40 20,20" fill="white" stroke="none"/>
            <polygon points="80,20 100,50 80,80 60,50" fill="white" stroke="none"/>
            <polygon points="50,60 80,80 50,100 20,80" fill="white" stroke="none"/> -->
    
            {#each Array(19) as _, i}
                {#each Array(19) as _, j}
                    <rect transform="translate({20 + (i-9) * 60} {20 + (j-9) * 60})" width="60" height="60" fill={getRectColor(i, j)}/>
                    <polygon transform="translate({20 + (i-9) * 60} {50 + (j-9) * 60})" points="0,-30 {offset},0 0,30 {-offset},0" fill={color1} stroke="none"/>
                    <polygon transform="translate({50 + (i-9) * 60} {20 + (j-9) * 60}) rotate(90)" points="0,-30 {offset},0 0,30 {-offset},0" fill={color2} stroke="none"/>
                    
                {/each}
            {/each}
    
            // <!-- <polygon transform="translate(500 500)" points="0,-30 20,0 0,30 -20,0" fill="white" stroke="none"/>
            // <polygon transform="translate(560 500)" points="0,-30 20,0 0,30 -20,0" fill="white" stroke="none"/>
            // <polygon transform="translate(620 500)" points="0,-30 20,0 0,30 -20,0" fill="white" stroke="none"/>
            // <polygon transform="translate(680 500)" points="0,-30 20,0 0,30 -20,0" fill="white" stroke="none"/> -->
            // <!-- <polygon transform="translate(530 470) rotate(90)" points="0,-30 20,0 0,30 -20,0" fill="white" stroke="none"/> -->
    
        </svg>
    </div>
    
    <style>
        #control {
          display: flex;
          width: 150px;
          justify-content: left;
          align-items: center;
        }
        input {
          width: 50%;
        }
    </style>
 