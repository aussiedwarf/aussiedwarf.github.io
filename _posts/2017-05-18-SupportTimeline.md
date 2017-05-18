---
layout: post
title: 'Support Timelines'
---

One of the things I find my self wondering when starting a project is what platforms and systems to support. Should I support older systems when their own vendor support may be about to expire? This leads to the question, how log will support last for the various systems?

So I have decided to try and put together the timelines (as wibbly wobbly timey winey as they are) for different systems. Hopefully I shall keep this up to date so that it cn be continually referenced.

There are many types of support offered by different organizations. For this I shall define 3 types that seem to cover the different types offered. One is full support, which should include added hardware, features and security support. Two is extended support, that includes security patches. Thirdly is paid support, typically only offered to those specifically paying for it and usually only includes security updates.

<canvas id="canvasTimelines" width="100" height="100" 
  style="border: 1px solid #e8e8e8;"></canvas>

<script>
$(document).ready(function() {
  var barHeight = 50;
  var c=document.getElementById("canvasTimelines");
  
  var width = window.innerWidth;
  var height = window.innerHeight - barHeight;

  //c.width = width * window.devicePixelRatio;
  //c.height = height * window.devicePixelRatio;
  
  //c.style.width  = width + 'px';
  //c.style.height = height + 'px';


  
  var jqxhr = $.getJSON( "/assets/support_timelines.json", function( data ) {
    
    var rowHeight = 20;
    var rowSpace = 20;
    var height = 0;
    var minDate = Date.now();
    var maxDate = Date.now();
    
    
    
    for(var i = 0; i < data.length; i++)
    {
      /*
      release":"2007-03-15",
      "mainstream_support":"2013-01-08",
      "extended_support":"2017-03-31",
      "private_support":"2020-11-30",
      */
      
      
      if(data[i].type == "OS")
      {
        for(var j = 0; j < data[i].data.length; j++)
        {
          height += rowHeight;
          
          var date;
          
          if(data[i].data[j].release)
          {
            date = Date.parse(data[i].data[j].release);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
          
          if(data[i].data[j].mainstream_support)
          {
            date = Date.parse(data[i].data[j].mainstream_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
          
          if(data[i].data[j].extended_support)
          {
            date = Date.parse(data[i].data[j].extended_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
          
          if(data[i].data[j].private_support)
          {
            date = Date.parse(data[i].data[j].private_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
        }
      }
      else if(data[i].type == "Browser")
      {
      
      }
    }
    
    
    
    width = window.innerWidth / 2;
    height = height + rowHeight*2;
    
    var dateWidth = width / (maxDate - minDate);

    c.width = width/* * window.devicePixelRatio*/;
    c.height = height/* * window.devicePixelRatio*/;
  
    c.style.width  = width + 'px';
    c.style.height = height + 'px';
    
    var ctx = c.getContext("2d");
    
    ctx.font="12px Verdana";
    ctx.lineWidth = 1;
    ctx.strokeStyle = "#b0b0b0";
    
    //Draw dates
    var maxDateYear = new Date(maxDate);
    var minDateYear = new Date(minDate);
    var numYears = maxDateYear.getFullYear() - minDateYear.getFullYear() + 2;
    var startYear = new Date(minDate);
    startYear.setFullYear(minDateYear.getFullYear() - 1);
    startYear.setMonth(0);
    startYear.setDate(0);
    for(var i = 0; i < numYears; i++)
    {
      var x = (startYear.getTime() - minDate)*dateWidth;
      
      
      ctx.beginPath();
      ctx.moveTo(x,0);
      ctx.lineTo(x,height);
      ctx.stroke();
      
      ctx.fillText(startYear.getFullYear(), x, rowHeight-3);
      
      startYear.setFullYear(startYear.getFullYear() + 1);
    }
    
    
    
    
    var pos = 0;
    var lineWidth = 4;
    
    ctx.font="14px Verdana";
    ctx.lineWidth = lineWidth;
    
    
    for(var i = 0; i < data.length; i++)
    {
    
      if(data[i].type == "OS")
      {
        for(var j = 0; j < data[i].data.length; j++)
        {

          var date;
          var left;
          var mid;
          
          pos += rowHeight;
          

          
          
          if(data[i].data[j].release)
          {
            date = Date.parse(data[i].data[j].release);
          
            left = (date - minDate) * dateWidth;
            
          
            ctx.strokeStyle = "red";
            ctx.fillStyle = "rgba(255,0,0,0.67)";
            
          
            if(data[i].data[j].mainstream_support)
            {
              date = Date.parse(data[i].data[j].mainstream_support);
              var w = (date - minDate) * dateWidth - left;
              
              ctx.fillRect(left+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(left+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = left + w;

            }
          
            if(data[i].data[j].extended_support)
            {
              ctx.fillStyle = "rgba(255,0,0,0.33)";
              
              date = Date.parse(data[i].data[j].extended_support);
              var w = (date - minDate) * dateWidth - mid;
              
              ctx.fillRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = mid + w;
            }
          
            if(data[i].data[j].private_support)
            {
              ctx.fillStyle = "rgba(255,0,0,0.0)";
              
              date = Date.parse(data[i].data[j].private_support);
              var w = (date - minDate) * dateWidth - mid;
              
              ctx.fillRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = mid + w;
          
            }
            
            ctx.fillStyle = "black";
            ctx.fillText(data[i].data[j].name, left + lineWidth, pos-lineWidth+rowHeight);
          
            
          }
        }
      }
      else if(data[i].type == "Browser")
      {
      
      }
    
    }
    
    var x = (Date.now() - minDate)* dateWidth;
    
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(x,0);
    ctx.lineTo(x,height);
    ctx.stroke();
    
  })
  
  

});
  
</script>
