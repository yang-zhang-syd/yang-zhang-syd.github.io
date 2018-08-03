---
layout: post
title:  "D3 Projection on Google Maps"
date:   2017-04-14 00:00:00 +1000
categories: tech blogs
---
### Summary
This blog explains how to use d3 to draw shapes on google maps and enable users to interact with them.
> You can find the source code on [**Plunker**](https://embed.plnkr.co/o5IuQV/)
### Problem
Use d3 to draw shapes, polygons, lines on the map at specified geo locations. Also, the d3 components need to be interactive which means the DOM events need to be captured and processed.

Usually, when using d3, SVG is used as the parent container for all d3 shapes. When drawing on the map all shapes are bound to geo-locations, which means the SVG need to grow and shrink as we zoom in or zoom out on the map and move the SVG with the map when we drag and drop. This introduces the challenge of correctly project geo-locations to d3 coordinates.
### Solution
Google Maps provides OverLayView to add custom layers to the map. We are going to draw our d3 shapes on one of those exposed google map layers.

According to Google Maps [MapPanes](https://developers.google.com/maps/documentation/javascript/reference#MapPanes) document, google maps exposed a few layer to customize. In our case, we need the d3 shapes to capture DOM events so we need to use overlayMouseTarget layer.

This blog will demonstrate drawing zones on google map and the code will be available in this [Plunker](https://embed.plnkr.co/o5IuQV/).

#### First, we add a custom OverLayView to google map.

{% highlight javascript %}
var overlay = new google.maps.OverlayView();
overlay.onAdd = function() {}
overlay.draw = function() {}
overlay.setMap(map);
{% endhighlight %}

#### Second, we define the projection help method.

{% highlight javascript %}
function googleProjection(prj) {
    return function(lnglat) {
        ret = prj.fromLatLngToDivPixel(new google.maps.LatLng(lnglat[1],lnglat[0]))
        return [ret.x, ret.y]
    };
}
{% endhighlight %}

The projection help method creates another function which is the actual projector method used to translate longitude and latitude pair to map coordinates. The prj parameter will be a utility object provided by google maps OverlayView.

#### Third, create an SVG element.

{% highlight javascript %}
var projection = googleProjection(overlay.getProjection());
var northWest = projection([139.499822,-27.183773]);
var southEast = projection([155.642935,-38.559921]);
var width = southEast[0] - northWest[0];
var height = southEast[1] - northWest[1];
svg = d3.select(overlay.getPanes().overlayMouseTarget)
    .append("svg")
    .style("position", "absolute")
    .style("top", northWest[1])
    .style("left", northWest[0])
    .attr("height", height)
    .attr("width", width);
{% endhighlight %}

To make d3 shapes interactive, we add the SVG layer on overlayMouseTarget layer defined by google map.

This is an important step because the SVG needs to be big enough to cover all the geo locations our data use. The code above we pass the latlng pairs which covering the entire NSW Australia to the projection method to get the North West and South East coordinates on map. Then, we calculate the width and height of the SVG. On creation of SVG, we pass the north-west coordinates as the top left coordinates on the page.

Based on your data coverage, you can choose the latlng pairs accordingly.

#### Fourth, get GeoJson data and draw shapes on the map.

{% highlight javascript %}
svg.selectAll("path")
    .data(data.features)
    .enter().append("path")
    .attr('transform', 'translate('+(-northWest[0])+' '+(-northWest[1])+')')
    .attr("d", path)
    .attr("fill", "#666666")
    .attr("fill-opacity", 0.3)
    .attr("stroke", "black");
{% endhighlight %}

Because we have set the top left of our SVG to the North East coordinates on map, when drawing shapes we need to translate the coordinates to reflect this. The following line of code does exactly what we want to achieve.

{% highlight javascript %}
      .attr('transform', 'translate('+(-northWest[0])+' '+(-northWest[1])+')')
{% endhighlight %}

#### Fifth, make shapes responsive to DOM events.

We will show a tool tip on mouse over the d3 shapes. Following is the tooltip element we create. Initially, it is hidden by setting style opacity to 0.

{% highlight javascript %}
var tipSvg = d3.select('body').append('div')
    .style('position', 'absolute')
    .style('max-width', '400px')
    .style('height', 'auto')
    .style('background-color', '#ffffff')
    .style('opacity', 0)
    .style('width', 600);
{% endhighlight %}

To make the shapes on map responsive we need to update the code drawing shapes as below.

{% highlight javascript %}
svg.selectAll("path")
    .data(data.features)
    .enter().append("path")
    .attr('transform', 'translate('+(-northWest[0])+' '+(-northWest[1])+')')
    .attr("d", path)
    .attr("fill", "#666666")
    .attr("fill-opacity", 0.3)
    .attr("stroke", "black")
    .on("mouseover", mapMouseOver)
    .on("mouseout", mapMouseOut);

function mapMouseOver(d) {
    var tip = '<p>' + d.properties.nsw_loca_2 + '</p>';
    tipSvg.html(tip)
        .style('left', (d3.event.pageX) + 'px')
        .style('top', (d3.event.pageY) + 'px');
    tipSvg.transition()
        .duration(500)
        .style('opacity', 1);
}

function mapMouseOut(d) {
    tipSvg.transition()
        .duration(500)
        .style('opacity', 0);
}
{% endhighlight %}

The result is that on mouse over the d3 shape the tooltip shows up with the name of the suburb. Then, the tooltip will be hidden again on mouse out.

### Reference
https://www.safaribooksonline.com/blog/2014/02/11/d3-js-maps/