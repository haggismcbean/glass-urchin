---
layout: post
title:  "D3 Collision Detection"
date:   2015-09-02 15:19:55
categories: jekyll update
---

D3 is a powerful javascript graphing library that I'm learning at the moment. There are good tutorials for most things already on the web but I found it wasn't that easy or intuitive to get some collision detection going. I'm using this for labels on the horizontal axis only as follows:

###Assign each label a unique id

    self.chart.selectAll("z")
      .data(data)
    .enter().append("text")
      .text(function(d) {
        return d.score + "%";
      })
      .attr("class", function(d, i){
        return "label-" + i;
      })

###Then you can get the width of the label like so

    var width = d3.select(".label-" + i).node().getBBox().width;

###Then we write a function to count how many labels we're overlapping:

    function positionOverlapping(d, i) {
        var width = d3.select(".label-" + i).node().getBBox().width;

        var left = self.scale(d.score) - 10 + self.margin / 2;
        var right = left + width;
        var overlap = 0;

        if (positions.length > 0){
            for (var j=0; j < positions.length; j++) {
                if (left > positions[j].right || right < positions[j].left) {
                    //no overlap
                } else {
                    overlap++;
                }
            }
        }

        var position = {
            right: right,
            left: left
        };

        positions.push(position)

        return overlap;
    }

###My collision detection is done within a function like this: 

    .attr("y", function(d, i) {
      var overlap = countOverlapping(d, i);
      return self.margin / 2 - offset - (15 * overlap);
    })

    