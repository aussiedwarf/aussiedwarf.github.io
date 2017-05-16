---
layout: post
title: 'Support Timelines'
---

One of the things I find my self wondering when starting a project is what platforms and systems to support. Should I support older systems when their own vendor support may be about to expire? This leads to the question, how log will support last for the various systems?

So I have decided to try and put together the timelines (as wibbly wobbly timey winey as they are) for different systems. Hopefully I shall keep this up to date so that it cn be continually referenced.

<canvas id="canvasTimelines" width="100" height="100"></canvas>

<script>
$(document).ready(function() {
  var barHeight = 50;
  var c=document.getElementById("canvasTimelines");
  
  var width = window.innerWidth;
  var height = window.innerHeight - barHeight;

  c.width = width * window.devicePixelRatio;
  c.height = height * window.devicePixelRatio;
  
  c.style.width  = width + 'px';
  c.style.height = height + 'px';


  
  var jqxhr = $.getJSON( "/assets/support_timelines.json", function( data ) {
    
    for(var i = 0; i < data.length; i++)
    {
      if(data[i].type == "OS")
      {
      
      }
    }
  })
  
  

});
  
</script>
