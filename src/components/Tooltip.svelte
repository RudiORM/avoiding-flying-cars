<script>
    export let data;
    export let innerWidth;
    export let height;
    export let step;

    let tooltipWidth;

    $: xPosition = 0.75*innerWidth
    $: tooltipWidth = 0.25*innerWidth


    import {fade } from "svelte/transition";
  </script>

  <div
    class="tooltip"
    transition={{fade}}
    style="position: absolute; opacity: {step>=5?1:0}; top: {0}px; right: {0}px; width: {tooltipWidth}px; height:{height}px"
    bind:clientHeight={height} 
  >    
{#if data.kind=='risk'}
    <h1>{data.node} </h1><span class='span2' style="background:{data.color+"EE"}">{'\u{26A0}'+' ' +data.group+' RISK'}</span>
    <h2><br>Risk ranking:  <a style="font-weight: 600">{1+data.rank_short}</a>/32 <br></h2>
    <br>
    <br>
    <h2><a style="font-weight: 600">{'Top Causes'}</a></h2>
    <br>
    {#each data.influences as node,index}
    <h3><span class='span3' style="background:{data.influences_col[index]}"></span>{node}</h3><br>
    {/each}
    <br>
    <h2><a style="font-weight: 600">{'Top Consequences'}</a></h2>
    <br>
    {#each data.influenced as node,index}
    <h3><span class='span3' style="background:{data.influenced_col[index]}"></span>{node}</h3><br>
    {/each}
{/if}

{#if data.kind=='ind'}
{#if data.r<50}
    <h1>{data.name} </h1><span class='span22' style="background:{data.color+"EE"}">{data.type}</span>

    <br><br>
    <!-- <span class='span2' style="background:{data.color+"EE"}">{'INNOVATION'}<br><br>AI and machine learning</span> -->
    {#if data.now.length>1}
    <h2>
      <a style = "font-weight:bold">Currently: </a>{data.now}</h2><br>

    {/if}

    {#if data['2025'].length>1}
    <h2>  <a style = "font-weight:bold">2025: </a>{data['2025']}</h2><br>
    {/if}


    {#if data['2030'].length>1}
    <h2>  <a style = "font-weight:bold">2030: </a>{data['2030']}</h2><br>
    {/if}




    <h2><a style="font-weight: 600">{'Top Short-Term Risks'}</a></h2>
    <br>
    {#each data.influenced as node,index}
    <h3><span class='span3' style="background:{data.influenced_col[index]}"></span>{node}</h3><br>
    {/each}

{/if}

{/if}

  </div>
  
  <style>
    .tooltip {
      padding: 28px 12px;
      background: #4d5061;
      pointer-events: none;
      transition: all 500ms;

    }
    h1,
    h2,h3 {
      margin: 2px 2px 2px 2px;
      padding: 0;
      font-weight: 300;
      color: whitesmoke;
    }
    h1 {
      font-size: 1.2rem;
      font-weight: 600;
      margin-bottom: 6px;
      width: 100%;
    }
    h2 {
      font-size: .9rem;
      margin-left: 0px;
    }

    h3 {
        margin-left: 10px;
      font-size: .9rem;
    }


    .span2 {
      /* background: #dda0dd50; */
      font-size: 80%;
      text-transform: uppercase;

      margin-right: 0px;
      margin-top: 0px;
      margin-top: 10px;
      margin-bottom: 10px;

      padding-left: 2px;
      padding-right: 2px;
      padding-top: 4px;
      padding-bottom:4px;
      text-align: center;
      display: inline-block;
      vertical-align: bottom;
      border-radius: 5px;
      color: white;
      width:97%;
    }


    
    .span22 {
      /* background: #dda0dd50; */
      font-size: 80%;
      text-transform: uppercase;

      margin-right: 0px;
      margin-top: 0px;
      margin-top: 10px;
      margin-bottom: 10px;

      padding-left: 2px;
      padding-right: 2px;
      padding-top: 4px;
      padding-bottom:4px;
      text-align: center;
      display: inline-block;
      vertical-align: bottom;
      border-radius: 5px;
      color: white;
      width:97%;
    }



    .span3 {
      /* background: #dda0dd50; */
      font-size: 80%;
      height: 10px;
      width:10px;
      margin-bottom: 2px;
      margin-right: 5px;
      margin-left: 0px;
      text-align: center;
      display: inline-block;
      vertical-align: bottom;
      border-radius: 5px;
      color: white;
    }

    .span4 {
      /* background: #dda0dd50; */
      font-size: 80%;
      height: 10px;
      width:50px;
      margin-bottom: 2px;
      margin-right: 3px;
      margin-left: 0px;
      text-align: center;
      display: inline-block;
      vertical-align: bottom;
      border-radius: 5px;
      color: white;
    }
  </style>