---
layout: post
title: 'Support Timelines'
comments: true
---

One of the things I find my self wondering when starting a project is what platforms and systems to support. Should I support older systems when their own vendor support may be about to expire? This leads to the question, how log will support last for the various systems?

So I have decided to try and put together the timelines (as wibbly wobbly timey winey as they are) for different systems. I keep this up to date so that it can be continually referenced.

There are many types of support offered by different organizations. For this I define 3 types that seem to cover the different types offered. One is full support (Green full shaded), which includes added hardware, features and security support. Two is extended support (Yellow half shaded), that includes security patches. Thirdly is paid support (Orange empty rectangle), typically only offered to those specifically paying for it and usually only includes security updates. Red is unsupported.

<canvas id="canvasTimelines" width="100" height="100" 
  style="border: 1px solid #e8e8e8;"></canvas>

<script>
function GetMinSupportedData(data, now)
{
  let minSupportedDate = now;

  for(let i = 0; i < data.length; i++)
  {
    if(data[i].type == "OS")
    {
      for(let j = 0; j < data[i].data.length; j++)
      {
        if(data[i].data[j].release)
        {
          const releaseDate = Date.parse(data[i].data[j].release);
          if (releaseDate < minSupportedDate)
          {
            if(data[i].data[j].mainstream_support)
            {
              const date = Date.parse(data[i].data[j].mainstream_support);
            
              if(date >= now)
              {
                minSupportedDate = releaseDate;
              }
            }
            
            if(data[i].data[j].extended_support)
            {
              const date = Date.parse(data[i].data[j].extended_support);
            
              if(date >= now)
              {
                minSupportedDate = releaseDate;
              }
            }
            
            if(data[i].data[j].private_support)
            {
              const date = Date.parse(data[i].data[j].private_support);
            
              if(date >= now)
              {
                minSupportedDate = releaseDate;
              }
            }

            if(data[i].data[j].enterprise_support)
            {
              const date = Date.parse(data[i].data[j].enterprise_support);
            
              if(date >= now)
              {
                minSupportedDate = releaseDate;
              }
            }

            if(data[i].data[j].private_enterprise_support)
            {
              const date = Date.parse(data[i].data[j].private_enterprise_support);
            
              if(date >= now)
              {
                minSupportedDate = releaseDate;
              }
            }
          }
        }
      }
    }
  }

  return minSupportedDate;
}

function CalculateNumRows(data, now, minSupportedDate)
{
  let rows = 0;

  for(let i = 0; i < data.length; i++)
  {
    if(data[i].type == "OS")
    {
      for(let j = 0; j < data[i].data.length; j++)
      {
        if(data[i].data[j].release)
        {
          let lastDate = Date.parse(data[i].data[j].release);
        
          if(data[i].data[j].mainstream_support)
          {
            const date = Date.parse(data[i].data[j].mainstream_support);
            if(date > lastDate)
              lastDate = date;
          }
          
          if(data[i].data[j].extended_support)
          {
            const date = Date.parse(data[i].data[j].extended_support);
            if(date > lastDate)
              lastDate = date;
          }
          
          if(data[i].data[j].private_support)
          {
            const date = Date.parse(data[i].data[j].private_support);
            if(date > lastDate)
              lastDate = date;
          }
          if(lastDate >= minSupportedDate)
          {
            ++rows;
          }
        }
        else
        {
          console.error("Missing release date for " + data[i].data[j].name);
        }
      }
    }

    return rows;
  }
}

$(document).ready(function() {
  var barHeight = 50;
  var c=document.getElementById("canvasTimelines");
  
  //var width = window.innerWidth;
  //var height = window.innerHeight - barHeight;

  //c.width = width * window.devicePixelRatio;
  //c.height = height * window.devicePixelRatio;
  
  //c.style.width  = width + 'px';
  //c.style.height = height + 'px';


  
  var jqxhr = $.getJSON( "/assets/support_timelines.json", function( data ) {
    
    const rowHeight = 20 * window.devicePixelRatio;
    const rowSpace = 20 * window.devicePixelRatio;
    const now = Date.now();
    let minDate = now;
    let maxDate = now;
    const minSupportedDate = GetMinSupportedData(data, now);
    const numRows = CalculateNumRows(data, now, minSupportedDate);
    
    const fontSizeData = 14 * window.devicePixelRatio;
    const fontSizeDate = 8 * window.devicePixelRatio
    
    for(let i = 0; i < data.length; i++)
    {
      if(data[i].type == "OS")
      {
        for(let j = 0; j < data[i].data.length; j++)
        {
          if(data[i].data[j].release)
          {
            const date = Date.parse(data[i].data[j].release);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;

            if(date < minSupportedDate)
            {
              continue;
            }
          }
          else
          {
            continue;
          }
          
          if(data[i].data[j].mainstream_support)
          {
            const date = Date.parse(data[i].data[j].mainstream_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
          
          if(data[i].data[j].extended_support)
          {
            const date = Date.parse(data[i].data[j].extended_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
          
          if(data[i].data[j].private_support)
          {
            const date = Date.parse(data[i].data[j].private_support);
          
            if(date < minDate)
              minDate = date;
            if(date > maxDate)
              maxDate = date;
          }
        }
      }
    }

    minDate = minSupportedDate;
    
    //width = window.innerWidth *2 / 4;
    const height = numRows * rowHeight + rowHeight*2;

    //c.width = width/* * window.devicePixelRatio*/;
    c.height = height/* * window.devicePixelRatio*/;

    const pixelHeight = height / window.devicePixelRatio;
  
    c.style.width  = '100%';
    c.style.height = pixelHeight + 'px';

    c.width = c.offsetWidth * window.devicePixelRatio;
    c.height = height;

    const dateWidth = c.width/ (maxDate - minDate);
    
    const ctx = c.getContext("2d");
    
    ctx.font="" + fontSizeDate + "px Verdana";
    ctx.lineWidth = 1;
    ctx.strokeStyle = "#b0b0b0";
    
    //create table
    const table = document.getElementById("tableTimelines");
    
    //Draw dates
    const maxDateYear = new Date(maxDate);
    const minDateYear = new Date(minDate);
    const numYears = maxDateYear.getFullYear() - minDateYear.getFullYear() + 2;
    const startYear = new Date(minDate);
    startYear.setFullYear(minDateYear.getFullYear() - 1);
    startYear.setMonth(0);
    startYear.setDate(0);
    for(let i = 0; i < numYears; i++)
    {
      const x = (startYear.getTime() - minDate)*dateWidth;
      
      ctx.beginPath();
      ctx.moveTo(x,0);
      ctx.lineTo(x,height);
      ctx.stroke();
      
      ctx.fillText(startYear.getFullYear()+1, x, rowHeight-3);
      
      startYear.setFullYear(startYear.getFullYear() + 1);
    }
    
    let pos = 0;
    let lineWidth = 4;
    let tableRow = 1;
    
    ctx.font="" + fontSizeData + "px Verdana";
    ctx.lineWidth = lineWidth;
    
    for(let i = 0; i < data.length; i++)
    {
      if(data[i].type == "OS")
      {
        for(let j = 0; j < data[i].data.length; j++)
        {
          let left;
          let mid;
          
          if(data[i].data[j].release)
          {
            const releaseDate = Date.parse(data[i].data[j].release);
            let lastDate = releaseDate;

            const row = table.insertRow(tableRow);
            const cell1 = row.insertCell(0);
            const cell2 = row.insertCell(1);
            const cell3 = row.insertCell(2);
            const cell4 = row.insertCell(3);
            const cell5 = row.insertCell(4);
            const cell6 = row.insertCell(5);
            
            tableRow++;

            cell1.innerHTML = data[i].data[j].name;
            cell2.innerHTML = data[i].data[j].release;
            cell6.innerHTML = "<a href='" + data[i].data[j].ref + "'>" + data[i].data[j].ref + "</a>";

            if (data[i].data[j].mainstream_support)
            {
              cell3.innerHTML = data[i].data[j].mainstream_support;
              lastDate = Date.parse(data[i].data[j].mainstream_support);
            }

            if (data[i].data[j].extended_support)
            {
              cell4.innerHTML = data[i].data[j].extended_support;
              lastDate = Date.parse(data[i].data[j].extended_support);
            }

            if (data[i].data[j].private_support)
            {
              cell5.innerHTML = data[i].data[j].private_support;
              lastDate = Date.parse(data[i].data[j].private_support);
            }

            if (lastDate < minSupportedDate)
            {
              continue;
            }

            pos += rowHeight;

            left = (releaseDate - minDate) * dateWidth;
            
            let extendedColor = "rgba(255,0,0,0.33)";
            let privateColor = "rgba(255,0,0,0.0)";
            if(now < Date.parse(data[i].data[j].mainstream_support))
            {
              ctx.strokeStyle = "green";
              ctx.fillStyle = "rgba(0,255,0,0.67)";
              extendedColor = "rgba(0,255,0,0.33)";
              privateColor = "rgba(0,255,0,0.0)";
            }
            else if(now < Date.parse(data[i].data[j].extended_support))
            {
              ctx.strokeStyle = "rgb(191,191,0)";
              ctx.fillStyle = "rgba(255,255,0,0.67)";
              extendedColor = "rgba(255,255,0,0.33)";
              privateColor = "rgba(255,255,0,0.0)";
            }
            else if(now < Date.parse(data[i].data[j].private_support))
            {
              ctx.strokeStyle = "orange";
              ctx.fillStyle = "rgba(255,127,0,0.67)";
              extendedColor = "rgba(255,127,0,0.33)";
              privateColor = "rgba(255,127,0,0.0)";
            }
            else{
              ctx.strokeStyle = "red";
              ctx.fillStyle = "rgba(255,0,0,0.67)";
            }
            
          
            if(data[i].data[j].mainstream_support)
            {
              const date = Date.parse(data[i].data[j].mainstream_support);
              const w = (date - minDate) * dateWidth - left;
              
              ctx.fillRect(left+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(left+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = left + w;
            }
          
            if(data[i].data[j].extended_support)
            {
              ctx.fillStyle = extendedColor;
              
              const date = Date.parse(data[i].data[j].extended_support);
              const w = (date - minDate) * dateWidth - mid;
              
              ctx.fillRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = mid + w;
            }
          
            if(data[i].data[j].private_support)
            {
              ctx.fillStyle = privateColor;
              
              const date = Date.parse(data[i].data[j].private_support);
              const w = (date - minDate) * dateWidth - mid;
              
              ctx.fillRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              ctx.strokeRect(mid+lineWidth/2,pos+lineWidth/2,w-lineWidth,rowHeight-lineWidth);
              
              mid = mid + w;
            }
            
            ctx.fillStyle = "black";
            ctx.fillText(data[i].data[j].name, Math.max(left + lineWidth, 0), pos-lineWidth+rowHeight);
          }
        }
      }
      else if(data[i].type == "Browser")
      {
      
      }
    
    }
    
    const x = (now - minDate)* dateWidth;
    
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(x,0);
    ctx.lineTo(x,height);
    ctx.stroke();

    ctx.lineWidth = lineWidth;

    ctx.strokeStyle = "green";
    ctx.fillStyle = "rgba(0,255,0,0.67)";
    ctx.fillRect(c.width*0.75, lineWidth/2+rowHeight*2, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.strokeRect(c.width*0.75, lineWidth/2+rowHeight*2, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.fillStyle = "black";
    ctx.fillText("Mainstream", c.width*0.75+4, -lineWidth+rowHeight*3);

    ctx.strokeStyle = "rgb(191,191,0)";
    ctx.fillStyle = "rgba(255,255,0,0.33)";
    ctx.fillRect(c.width*0.75, lineWidth/2+rowHeight*3, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.strokeRect(c.width*0.75, lineWidth/2+rowHeight*3, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.fillStyle = "black";
    ctx.fillText("Extended", c.width*0.75+4, -lineWidth+rowHeight*4);

    ctx.strokeStyle = "orange";
    ctx.fillStyle = "rgba(255,127,0,0.0)";
    ctx.fillRect(c.width*0.75, lineWidth/2+rowHeight*4, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.strokeRect(c.width*0.75, lineWidth/2+rowHeight*4, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.fillStyle = "black";
    ctx.fillText("Private", c.width*0.75+4, -lineWidth+rowHeight*5);

    ctx.strokeStyle = "red";
    ctx.fillStyle = "rgba(255,0,0,0.67)";
    ctx.fillRect(c.width*0.75, lineWidth/2+rowHeight*5, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.strokeRect(c.width*0.75, lineWidth/2+rowHeight*5, c.width/4-lineWidth, rowHeight-lineWidth);
    ctx.fillStyle = "black";
    ctx.fillText("Unsupported", c.width*0.75+4, -lineWidth+rowHeight*6);
    
  })
  
});
  
</script>

<table id="tableTimelines">
  <tr>
    <th>Name</th>
    <th>Release</th>
    <th>Full</th>
    <th>Extended</th>
    <th>Private</th>
    <th>Reference</th>
  </tr>
</table>
