---
layout: project
title: JSNetworkX
link: http://felix-kling.de/JSNetworkX
source: https://github.com/fkling/JSNetworkX
categories:
    - JavaScript
    - HTML5
    - Graph processing
---

JSNetworkX is a JavaScript port of the Python graph library [NetworkX](http://networkx.lanl.gov/). From their web site:

> NetworkX is a Python language software package for the creation, manipulation, and study of the structure, dynamics, and function of complex networks.
> 
> With NetworkX you can load and store networks in standard and nonstandard data formats, generate many types of random and classic networks, analyze network structure, build network models, design new network algorithms, draw networks, and much more.

The development is still in an early stage, but more and more parts of the library
are ported every day.


<script src="/assets/js/jquery-1.7.2.min.js"> </script>
<script src="/assets/js/d3.v2.min.js"> </script>
<script src="/assets/js/jsnetworkx.js"> </script>
<script id="jsnx_anchor">
    var d = $('<div />').appendTo(
        $("#jsnx_anchor").closest('.project').find('.picture')
    ).addClass('image').height(180).width(180).attr('id', 'chart');

(function() {

    // Ordinal color function
    // See the D3 documentation
    var color = d3.scale.category10();

    // *Experimental* graph draw function
    // This will make it into the library eventually, after
    // it has been given a bit more thought.
    //
    // Draw your own graph by opening the console. E.g
    //
    // > G = new jsnx.Graph();
    // > G.add_edge(10,12);
    // > draw_jsnx_graph(G, '#chart');
    //
    // See below for an example of attr data.
    //
    var draw_jsnx_graph = function(G, element, attr) {
        attr = attr || {};
        d3.select(element).select('svg').remove();
        var color = d3.scale.category20(),
        width = attr.width || parseInt(d3.select(element).style('width'), 10),
        height = attr.height || parseInt(d3.select(element).style('height'), 10),
        vis = d3.select(element).append('svg').attr('width', width).attr('height', height);


        vis.style("opacity", 1e-6)
        .transition()
        .duration(1000)
        .style("opacity", 1);

        var nodes = [], nodes_map = {}, edges_map = {}, edges = [];
        jsnx.forEach(G.nbunch_iter(attr.nodes), function(n) {
            var index = nodes.push({label: n, data: G.node[n]});
            nodes_map[n] = index - 1;
        });

        jsnx.forEach(G.edges_iter(attr.nodes, true), function(edg) {
            edges_map[edg] = 1;
            if(edg[1] in nodes_map) {
                edges.push({source: nodes_map[edg[0]], target: nodes_map[edg[1]], data: edg[2]});
            }  
        });

        var na = attr.node_attr || {},
        ns = attr.node_style || {},
        ea = attr.edge_attr || {},
        es = attr.edge_style || {};


        na.r = na.r || 5;
        ns.fill = ns.fill || '#000';
        es.stroke = es.stroke || '#999';


        var force = d3.layout.force()
        .charge(-60)
        .nodes(nodes)
        .links(edges)
        .size([width, height])
        .start();

        force.on("tick", function() {
            vis.selectAll("line")
            .attr("x1", function(d) { return d.source.x; })
            .attr("y1", function(d) { return d.source.y; })
            .attr("x2", function(d) { return d.target.x; })
            .attr("y2", function(d) { return d.target.y; });

            vis.selectAll("circle")
            .attr("cx", function(d) { return d.x; })
            .attr("cy", function(d) { return d.y; });
        });


        var lines = vis.selectAll("line")
        .data(edges)
        .enter().append("line");

        jsnx.forEach(ea, function(k) {
            lines.attr(k, ea[k]);
        });

        jsnx.forEach(es, function(k) {
            lines.style(k, es[k]);
        });


        var circles = vis.selectAll("circle")
        .data(nodes)
        .enter().append("circle")
        .call(force.drag);

        jsnx.forEach(na, function(k) {
            circles.attr(k, na[k]);
        });

        jsnx.forEach(ns, function(k) {
            circles.style(k, ns[k]);
        });
    };

    // Creating the example graph 
    var G = jsnx.Graph();
    G.add_nodes_from([1,2,3,4], {group:0});
    G.add_nodes_from([5,6,7], {group:1});
    G.add_nodes_from([8,9,10,11], {group:2});

    var star = [20,21,22,23,24,25,26,27];
    G.add_nodes_from(star, {group: 3});
    G.add_star(star);


    G.add_path([1,2,5,6,7,8,11]);
    G.add_edges_from([[1,3],[1,4],[3,4],[2,3],[2,4],[8,9],[8,10],[9,10],[11,10],[11,9]]);

    // Draw graph
    draw_jsnx_graph(G, '#chart', {
        node_attr: {
            r: 5,
            title: function(d) { return d.label;}
        },
        node_style: {
            fill: function(d) { return color(d.data.group); }
        },
        edge_style: {
            stroke: '#999'
        }
    });
}());
</script>
