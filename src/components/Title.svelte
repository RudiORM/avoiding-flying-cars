<script>
    export let innerWidth;
    export let innerHeight;
    export let step;

    import {fade } from "svelte/transition";
  </script>

  <div
    class="title"
    transition={fade}
    style="position: absolute; opacity: {step>-1?0:1}; top: {innerHeight/3}px; left: {0}px; width: {innerWidth}px; "
  >    

    <h1>Avoiding Flying Cars</h1>
    <br>
    <br>
    <br>
    <h2>Rudi O'Reilly Meehan</h2>
    <br>
    <br>
    <br>
<br>
    <br>
    <h3>Scroll down to begin.</h3>


  

  </div>
  
  <style>
    .title {
      padding: 28px 12px;
      pointer-events: none;
      transition: ease 500ms;
      z-index: 100;

    }
    h1,
    h2,h3 {
      margin: 2px 2px 2px 2px;
      padding: 0;
      font-weight: 300;
      color: whitesmoke;
      text-align: center;
    }
    h1 {
      font-size: 4rem;
      font-weight: 600;
      margin-bottom: 6px;
      padding-bottom: 6px;
      width: 100%;
      background-color: #30323Dcc;
    }
    h2 {
      font-size: 1.4rem;
      margin-left: 0px;
      padding-bottom: 6px;
      background-color: #30323Dcc;

    }

    h3 {
      font-size: 1rem;
      background-color: #30323Dcc;
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
      width:98%;
    }


    .span22 {
      /* background: #dda0dd50; */
      font-size: 80%;

      margin-right: 0px;
      margin-top: 0px;
      margin-top: 10px;
      margin-bottom: 10px;
      line-height: 1.4em;
      padding-left: 6px;
      padding-right: 6px;
      padding-top: 4px;
      padding-bottom:4px;
      text-align: left;
      display: inline-block;
      vertical-align: bottom;
      border-radius: 5px;
      color: white;
      background-color: #30323D;
      width:93%;
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