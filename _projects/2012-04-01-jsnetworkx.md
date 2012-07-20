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

The development is still in an early stage, but the base graph classes and drawing
is available.

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
    jsnx.draw(G, {
        element: '#chart',
        layout_attr: {
            linkDistance: 20,
            charge: -60
        },
        node_attr: {
            r: 5,
            title: function(d) { return d.label;}
        },
        node_style: {
            fill: function(d) { return color(d.data.group); }
        },
        edge_style: {
            stroke: '#999'
        },
        pan_zoom: {
            enabled: false, 
            scale: false
        }
    });
}());
</script>
