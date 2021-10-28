<template>
  <div class="hello">
    <div :id="id"></div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "Netowrk",
  props: {
    msg: String,
    graph: Object,
    id: String,
  },
  mounted() {
    var width = "100%",
      height = "100%";
    d3.select("#"+this.id).selectAll("svg").remove();
    var svg = d3
      .select("#"+this.id)
      .append("svg")
      .attr("width", width)
      .attr("height", height)
      .call(
        d3.zoom().on("zoom", function (event) {
          svg.attr("transform", event.transform);
        })
      )
      .append("g");

      var simulation = d3
        .forceSimulation()
        .force(
            "link",
            d3
            .forceLink()
            .iterations(1)
            .id(function(d) {
                return d.id;
            })
        )
        .force("charge", d3.forceManyBody().strength(-100))
        .force("x", d3.forceX(500))
        .force("y", d3.forceY(250));

    var link = svg
        .append("g")
        .attr("class", "links")
        .selectAll("line")
        .data(this.graph.links)
        .enter()
        .append("line")
        .attr("stroke-width", 2)
        .attr("stroke", "#999")
        .attr("stroke-opacity", "0.6");

    var node = svg
        .append("g")
        .attr("class", "nodes")
        .selectAll("g")
        .data(this.graph.nodes)
        .enter()
        .append("g").call(drag(simulation));

    var circles = node
        .append("circle")
        .attr("r", 7)
        .attr("stroke", "red")
        .attr("stroke-width", "1.5px")
        

    var lables = node
        .append("text")
        .attr("font-family", "sans-serif")
        .attr("font-size", "10px")
        .text(function(d) {
            
                return d.id;
         })
        .attr("x", 6)
        .attr("y", 3);
    console.log(lables,circles)
    node.append("title").text(function(d) {
        return d.id;
    });

    simulation.nodes(this.graph.nodes).on("tick", ticked);

    simulation.force("link").links(this.graph.links);

    function ticked() {
        link
            .attr("x1", function(d) {
                return d.source.x;
            })
            .attr("y1", function(d) {
                return d.source.y;
            })
            .attr("x2", function(d) {
                return d.target.x;
            })
            .attr("y2", function(d) {
                return d.target.y;
            });

        node.attr("transform", function(d) {
            return "translate(" + d.x + "," + d.y + ")";
        });
    }

   function drag(simulation) {    
    function dragstarted(event) {
      if (!event.active) simulation.alphaTarget(0.3).restart();
      event.subject.fx = event.subject.x;
      event.subject.fy = event.subject.y;
    }
    
    function dragged(event) {
      event.subject.fx = event.x;
      event.subject.fy = event.y;
    }
    
    function dragended(event) {
      if (!event.active) simulation.alphaTarget(0);
      event.subject.fx = null;
      event.subject.fy = null;
    }
    
    return d3.drag()
      .on("start", dragstarted)
      .on("drag", dragged)
      .on("end", dragended);
  }


   

    

  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
