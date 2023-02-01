<script>
    export let colorScale;
    export let step;
  </script>
  
  <div class='legend' style="opacity:{(step>=3&&step<14)?1:0};width:{step<6?'100vw':'70vw'}">
    {#each colorScale.domain() as continent}
        <p>
            <span style="background-color: {colorScale(continent)}">
            </span>
            {continent}
        </p>
    {/each}
  </div>
  
  <style>
    .legend {
        position: absolute;
        top: 93vh;
        display: flex;
        
      justify-content: center;
      flex-direction: row;
      flex-wrap: wrap;
      column-gap: 15px;
      row-gap: 5px;
      margin-bottom: 0.25rem;
      z-index: 99;
      transition: all 500ms;
    }
    p {
      margin: 0;
      font-size: 0.8rem;
      text-transform: uppercase;
      display: flex;
      color: whitesmoke;
      align-items: center;
      column-gap: 3px;
    }
    span {
      width: 9px;
      height: 9px;
      display: inline-block;
      border-radius: 50%;
      border: 1px solid rgba(0, 0, 0, 0.5);
    }
  </style>