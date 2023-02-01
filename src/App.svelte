<script>

import data from './data/node_data'
import Scrolly from "./Scrolly.svelte";
import { fly } from "svelte/transition";
import Tooltip from "$components/Tooltip.svelte";
import Title from "$components/Title.svelte";

import {scaleLinear, scaleOrdinal, scaleSqrt } from "d3-scale";
import { tweened } from "svelte/motion";
import { backIn, backInOut, backOut, cubicIn, cubicInOut, cubicOut, elasticIn, elasticOut, expoIn, expoOut } from "svelte/easing";
import Image from './components/Image.svelte'
import Legend from "$components/Legend.svelte";

const tweenOptions = {
    delay: 0,
    duration: 1500,
    // easing: backInOut
    easing: cubicOut

  };
  
let width = 600
let height = 300;

let keys = ['Environmental', 'Geopolitical', 'Economic', 'Technological', 'Societal']
let cols = ['#498467', '#16A9A4', '#5c80bc', '#ED6C43', '#7376AD']

let colorScale = scaleOrdinal()
    .domain(keys) // continents was already defined in our code
    .range(cols);


$: h2 = height;

const margin = { top: 10, right: 10, bottom: 10, left: 10 };

$: innerWidth = width - margin.left - margin.right
$: innerHeight = height - margin.top - margin.bottom

let tooltip_over = 1

let step=-1;

let hovered = data.nodes[4];

$: is_hovered = hovered==data.nodes[4]?0:1

let kind = 'risk'



$: xScale = scaleLinear()
    .range([0.05*innerWidth,0.95*innerWidth])
    .domain([0, 1]);

$: yScale = scaleLinear()
    .range([0.9*innerHeight, .07*innerHeight])
    .domain([0, 1]);


 $: xoff = (0.7*innerWidth)>innerHeight? ((0.7*innerWidth)-innerHeight)/2 :0
 $: yoff = (0.7*innerWidth)<innerHeight? (innerHeight-(0.7*innerWidth))/2 -2:-0.02*innerHeight


$: xScale2 = scaleLinear()
.range([xoff,xoff+Math.min(0.7*innerWidth,innerHeight)])
    .domain([-1, 1]);

$: yScale2 = scaleLinear()
.range([yoff+Math.min(.7*innerWidth,innerHeight),yoff])
    .domain([-1, 1]);


$: xoff2 = (innerWidth)>innerHeight? ((innerWidth)-innerHeight)/2 :0
$: yoff2 = (innerWidth)<innerHeight? (innerHeight-(innerWidth))/2 :-0.02*innerHeight


    $: xScale22 = scaleLinear()
.range([xoff2,xoff2+Math.min(innerWidth,innerHeight)])
    .domain([-1.05, 1.05]);

$: yScale22 = scaleLinear()
.range([yoff2+Math.min(innerWidth,innerHeight),yoff2])
    .domain([-1.05, 1.05]);




$: xScale3 = scaleLinear()
.range([xoff,xoff+Math.min(.7*innerWidth,innerHeight)])
    .domain([-350, 350]);

$: yScale3 = scaleLinear()
    .range([yoff+Math.min(.7*innerWidth,innerHeight),yoff])
    .domain([-350, 350]);


$: rScale = scaleSqrt()
    .domain([0.5,3])
    .range([0, Math.min(innerWidth,innerHeight)/45]);

    $: rScale2 = scaleLinear()
    .domain([0,700])
    .range([0, Math.min(0.7*innerWidth,innerHeight)/1]);


$: linkScale = scaleLinear()
.range([0,Math.min(innerWidth,innerHeight)/100]
)
.domain([0,Math.max(...data.links.map(o => o.weight))]
)

$: xScale4 = scaleLinear()
    .range([0.05*innerWidth,0.7*innerWidth])
    .domain([0, 1]);


let narrative = 


['In the 1960s, futurists and science fiction writers were certain there would be flying cars in the near future.<br><br> In his 2018 book, <a style = "font-weight:bold">Where Is My Flying Car?</a>, author J. Storrs Hall looked to answer how they got their predictions so... wrong? <br><br>This led Hall to investigate the technological and social roots of the economic stagnation that started in the 1970s, and how this derailed our collective ambitions for the future.',

"As was the case in the 1970s, we are currently experiencing an energy crisis in Europe, ever-rising inflation and a cost-of-living crisis. <br><br>As such, when considering the innovations and developments that we believe will shape our future society, how do we make sure we are <a style = 'font-weight:bold'> avoiding flying cars?</a>",

"One potential way of making more robust predictions is by considering the risks facing the world in 2023 and how these risks could shape future developments and innovations.<br><br> This does not mean pessimism – rather, it is about making sure our ideas are grounded in reality and can survive the near-term risks facing the world today. And a great place to start with understanding these risks is a recent report by the World Economic Forum.",

"The WEF’s 2023 edition of its  <a style = 'font-weight:bold'>Global Risks Report</a> was released in January 2023.<br><br> This report provides an overview of the  <a style = 'font-weight:bold'> 32 most pressing global risks that the world is facing</a>, as identified by over 1,200 experts. <br><br>In fact, every circle you see on this diagram corresponds to a different risk, with larger circles indicating a more severe risk. Risks are split into 5 categories, as shown on the legend below.",

"Experts were asked to estimate the relative impact of the risks over a short-term period of 2 years. <br><br>The <a style = 'font-weight:bold'>cost-of-living crisis</a> was the most severe risk, with <a style = 'font-weight:bold'>natural disasters</a> and <a style = 'font-weight:bold'>geoecoomic confrontation</a> rounding out the top 3.",

"A key point is that <a style = 'font-weight:bold'> risks do not exist in a vacuum. </a> Instead, many risks are inextricably linked. <br><br>For example, let’s consider the risk of <a style = 'font-weight:bold'>large-scale involuntary migration</a>. <br><br>This risk could occur as a result of <a style = 'font-weight:bold'> natural disasters </a> or <a style = 'font-weight:bold'>state collapse</a>.<br><br> This risk could also lead to a proliferation of other risks such as <a style = 'font-weight:bold'>societal polarisation.</a> ",

"In order to find which risks were linked, experts were asked to select the top causes and consequences of every risk. This results in a <a style = 'font-weight:bold'>risks systems map</a> – a series of interconnected risks.",

"Hovering over a circle on this systems map reveals information about the risk and its connected risks.",

"Next, let’s bring in some future innovations and consider how these concepts could be impacted by global risks.",

"A dataset of innovations and developments was provided by the World Government Summit, containing predictions of progress against <a style = 'font-weight:bold'>39 future innovations and developments</a>. ",

"Let’s pick one such development: <a style = 'font-weight:bold'>Megacities</a>. What current risks could affect innovations in this space? <br><br>To this end, I have performed a rough mapping exercise between <a style = 'font-weight:bold'>Megacities</a> and global risks.",

"For example, an <a style = 'font-weight:bold'>Asset bubble burst</a> in Chinese housing has led to “ghost cities” with millions of dormant buildings. <br><br><a style = 'font-weight:bold'>Employment crises</a> and an increased <a style = 'font-weight:bold'>cost of living </a>could make megacities unaffordable for the majority. <br><br>These megacities may also result in <a style = 'font-weight:bold'>digital inequality</a> and a growing urban-rural divide.",

"You might not agree with these choices of risks, and that is exactly the intention here. With this exercise, the intention is to stress-test and debating these innovations so we can ultimately create more robust versions.<br><br> For example, how could we create megacities that do not result in increased digital inequality?",

"Let’s bring in the rest of these innovations, which have also been mapped to global risks. <br><br>Hover over an innovation for a description around how it is predicted to be used in the next 10 years, along with potential short-term risks that might be associated with it.<br><br>Can you consider ways to protect these innovations from some of these risks?",

"To conclude, the methods within this piece are examples of Systems Thinking, which Barry Richmond describes as <a style = 'font-weight:bold'>\"Seeing both the forest and the trees; one eye on each\"<\a>.<br><br>I hope that in doing this, you can avoid flying cars.<br><br>Thank you!"


]



let initialData = data.nodes;
let initialLinks = data.links;
let tmpdata

console.log(initialData)


  $: tweenedX = tweened(initialData.map((d) => xScale(0.5)),tweenOptions);
  $: tweenedY = tweened(initialData.map((d) => yScale(0.5)),tweenOptions);

  $: tweenedX1 = tweened(initialLinks.map((d) => xScale(0.5)),tweenOptions);
  $: tweenedY1 = tweened(initialLinks.map((d) => yScale(0.5)),tweenOptions);
  $: tweenedX2 = tweened(initialLinks.map((d) => xScale(0.5)),tweenOptions);
  $: tweenedY2 = tweened(initialLinks.map((d) => yScale(0.5)),tweenOptions);



 let short_long = 'short'


  $: tweenedData = initialData.map((d, index) => ({
    x: $tweenedX[index],
    y: $tweenedY[index]
  }));


  $: tweenedLink = initialLinks.map((d, index) => ({
    x1: $tweenedX1[index],
    y1: $tweenedY1[index],
    x2: $tweenedX2[index],
    y2: $tweenedY2[index]
  }));


$: {
  

    if (step <2) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));

    }

    if (step == 1) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale22(d.cpx)));
      tweenedY.set(initialData.map((d) => yScale22(d.cpy)));

      tweenedX1.set(initialLinks.map((d) => xScale22(d.s_cpx)));
      tweenedY1.set(initialLinks.map((d) => yScale22(d.s_cpy)));
      tweenedX2.set(initialLinks.map((d) => xScale22(d.t_cpx)));
      tweenedY2.set(initialLinks.map((d) => yScale22(d.t_cpy)));

    }


    if (step ==2) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));

    }

   
  
    if (step == 4) {
      short_long = 'short'
      tweenedX.set(initialData.map((d,i) => xScale(0.05+0.5*(Math.floor(d.rank_short/16)))));
      tweenedY.set(initialData.map((d,i) => yScale(.96-0.06*(d.rank_short%16))));

      tweenedX1.set(initialLinks.map((d,i) => xScale(0.05+0.5*(Math.floor(d.source_risk_short/16)))));
      tweenedY1.set(initialLinks.map((d,i) => yScale(.96-0.06*(d.source_risk_short%16))));
      tweenedX2.set(initialLinks.map((d,i) => xScale(0.05+0.5*(Math.floor(d.source_risk_short/16)))));
      tweenedY2.set(initialLinks.map((d,i) => yScale(.96-0.06*(d.source_risk_short%16))));

    }


    // if (step == 4) {
    //   short_long = 'long'
    //   tweenedX.set(initialData.map((d,i) => xScale(0.05+0.5*(Math.floor(d.rank_long/16)))));
    //   tweenedY.set(initialData.map((d,i) => yScale(.96-0.06*(d.rank_long%16))));



    // }

    if (step == 5) {
      short_long = 'short'
      hovered = data.nodes[4];
      tweenedX.set(initialData.map((d) => xScale(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));

    }

    if (step == 6) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale4(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale4(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale4(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));
    }





    if (step == 7) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale4(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale4(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale4(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));
    }


    if (step >= 8) {
      short_long = 'short'
      tweenedX.set(initialData.map((d) => xScale2(d.cpx)));
      tweenedY.set(initialData.map((d) => yScale2(d.cpy)));

      tweenedX1.set(initialLinks.map((d) => xScale2(d.s_cpx)));
      tweenedY1.set(initialLinks.map((d) => yScale2(d.s_cpy)));
      tweenedX2.set(initialLinks.map((d) => xScale2(d.t_cpx)));
      tweenedY2.set(initialLinks.map((d) => yScale2(d.t_cpy)));

    }

    if (step == 10) {
      short_long = 'short'
     hovered = data.packs[36]

    }

    if (step ==14) {
      hovered = data.nodes[4];

      tweenedX.set(initialData.map((d) => xScale(d.cx)));
      tweenedY.set(initialData.map((d) => yScale(d.cy)));

      tweenedX1.set(initialLinks.map((d) => xScale(d.s_cx)));
      tweenedY1.set(initialLinks.map((d) => yScale(d.s_cy)));
      tweenedX2.set(initialLinks.map((d) => xScale(d.t_cx)));
      tweenedY2.set(initialLinks.map((d) => yScale(d.t_cy)));

    }


  }



</script>

<section>



  
  <div class = 'sticky'>

   

  <div class='chart-container' 
  bind:clientWidth={width} 
  bind:clientHeight={height}>
    <svg width={(width-1)} height={height-3}>
      <g class="inner-chart" transform="translate({margin.left}, {margin.top})">
        {#each tweenedLink as link,index}
				<line class='link' 
        x1={(link.x1)} 
        y1={(link.y1)} 
        x2={(link.x2)} 
        y2={(link.y2)} 
        stroke={initialLinks[index].color}
        opacity={hovered.kind=='risk'? (step>=5? 
       (step==5?(hovered.highlight_links.includes(index)? 1: 0):
       (hovered.highlight_links.includes(index)? 1: 0.1)): 0.1):0
}

        style="stroke-width:{linkScale(initialLinks[index].weight)}px" />
    	{/each} 

        {#each tweenedData as node,index}
        <circle cx={(node.x)}
        cy={(node.y)}
        r={short_long == 'short'? rScale(initialData[index].r_short): rScale(initialData[index].r_long)}
        fill={initialData[index].color}
        on:mouseover={() => (step>5&&step<10)? hovered = initialData[index]:(step>12?hovered = initialData[index]:hovered)}
        on:focus={() => step>5? hovered = initialData[index]:hovered}
        opacity={




          hovered.kind=='risk'?((step>=5&&step<6)?(hovered.highlight_nodes.includes(index)? 1: 0):step>6?(hovered.highlight_nodes.includes(index)?1:1):1):hovered.influenced.includes(initialData[index].node)?1:0.05} 


        
        
        />





        <text x={short_long == 'short'? node.x +7+ rScale(initialData[index].r_short):node.x +7+ rScale(initialData[index].r_long)} 
        y={node.y} width = 30px dy='.35em' 
        style="font-size:{(11/800)*innerWidth}px"
        opacity={(step==4)?
        1:(
          (step==5&&(['Natural disasters and extreme weather events','State collapse or severe instability','Erosion of social cohesion and societal polarisation','Large-scale involuntary migration'].includes(initialData[index].node)))?1:0)}> 

        {(short_long=='short'? 1+initialData[index].rank_short+': '+initialData[index].node:1+initialData[index].rank_long+': '+initialData[index].node)}</text>
        {/each}
      </g>

      <g class="inner-chart2" transform="translate({margin.left}, {margin.top})">

        {#each data.packs as node,index}
        <circle cx={xScale3(node.cx)}
        cy={yScale3(node.cy)}
        r={(step>=9&&step<14)? rScale2(node.r):0}
        fill={node.color}
        opacity={
        (step>=10)?(node.opa==1?(node['name']==hovered['name']?1:0.25):node.opa):node.opa
        }

        on:mouseover={() => (step>=13 && node.r<50)? hovered = node:hovered}
        on:focus={() =>(step>=13 && node.r<50)? hovered = node:hovered}
/>
        {/each}

    

      <text></text>

      </g>
   
    </svg>


  </div>

  <Title {innerWidth}{innerHeight}{step}
    />
  
  <div class = 'banner-container'>

  </div>

  <Legend {colorScale} {step}
    />

    {#if [7,8,11,12,13].includes(step)} 

    <Tooltip data={hovered} {innerWidth}{height}{step} />
  
    
    {/if}
  

  

</div>



<div class='steps'>

 

  <div>
  <Scrolly bind:value={step}>
    {#each narrative as text, i}
    <div class="step" class:active={step === i} style="margin-right:{(step>=6&step<14)?25:0}vw"
          >
            <div class='step-content'>
              {@html text}
              </div>
          </div>
      {/each}
  </Scrolly>
</div>



</section>


<style>

@import url('https://fonts.googleapis.com/css2?family=Jost:wght@300;600&display=swap');


  .tooltip{
    position: absolute;
    top: 0px;
  
    pointer-events: none;
    z-index: 99 !important;
  }

  section{
    background-color: #30323D;
    font-family: "Jost";
 

  }

  .chart-container {
margin: auto;
height: 100vh;
background-color: #30323D;
transition: opacity 500ms;
overflow-x: hidden;
max-height: 100%;


  }



  .banner-container {
    background-image: url("./WGS-summit-logo.svg");
    background-repeat: no-repeat;
    background-size: 100% auto;
    position: absolute;
    overflow-y: hidden;
    top: 4px;
    left: 4px;
    z-index: 100;
    height: 30px;
    width: 110px;
}


 
line {
  stroke-opacity: 1;
  transition: r 750ms ease, opacity 750ms ease;

}

circle {
    transition: r 750ms ease, opacity 750ms ease;
    cursor: pointer;
}



/* 
  circle {
    transition: cx 300ms ease-in-out, cy 300ms ease-in-out, r 300ms ease, opacity 500ms ease;
    cursor: pointer;
  } */


  /* circle {
    transition: r 300ms ease, 
        opacity 500ms ease, 
        cx 750ms cubic-bezier(0.76, 0, 0.24, 1), 
        cy 750ms cubic-bezier(0.76, 0, 0.24, 1);
} */





  .step {
    height: 200vh;
    opacity: 0.3;
    transition: opacity 300ms ease;
     display: flex; /* ✅ */
    justify-content: center; /* ✅ */
    place-items: center; /* ✅ */
    overflow-x: hidden;
    pointer-events: none;

  }
  .step.active {
    opacity: 1;
  }


  .step-content {
    padding: 0.75rem 1rem 0.75rem 1rem;
    max-width: 45vw;
    font-size: 1.2rem;
    pointer-events: none;
    color: whitesmoke;
    line-height: 1.4em;
    word-wrap: break-word;
    z-index: 3;
    background:#4d5061dd;
    border-radius: 10px;
    


  }




  section {
    position: relative;
  }
  .sticky {
    position: sticky;
    top: 0;
    left: 20vw;
    z-index: 1;
    max-width: 100%;
    max-height: 100%;
  }


  .steps {
    z-index: 2;
    position: relative;
    pointer-events: none;
    overflow: hidden;
  }







  text{
    text-anchor: start;
    pointer-events: none;
    fill: white;
    word-wrap: break-word;
  }



a {font-weight: 600}



</style>
