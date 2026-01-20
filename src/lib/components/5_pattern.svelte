<script>
    import chroma from 'chroma-js';
    
    const squareCount = 20;
    const squareSize = 1000 / squareCount;
    
    let offset = $state(0);
    let hue = $state(210);
    let color1 = $derived(chroma.oklch(0.82, 0.4, hue));
    let color2 = $derived(chroma.oklch(0.82, 0.4, 260));
    let color3 = $derived(chroma.oklch(1, 0.1, hue));
    let color4 = $derived(chroma.oklch(1, 0.11, 260));
    
     
    // function getColor(xi, yi) {
    //     if (xi % 2 === 0 && yi % 2 === 0) {
    //       return color1.hex();
    //     } else if (xi % 2 === 1 && yi % 2 === 0) {
    //       return color2.hex();
    // 	}
    // }
        
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
        <input id="hue" type="range" min="1" max="360" bind:value={hue} />
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