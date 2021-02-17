<script>
    import { forceData } from "../store.js";
    import * as d3 from "d3";
    import { onMount } from "svelte";
    import { getLinkColor } from "../helpers/GetLinkColor.svelte";
    import { getNeighbors } from "../helpers/GetNeighbors.svelte";
    import { getNodeColor } from "../helpers/GetNodeColor.svelte";
    import { getTextColor } from "../helpers/GetTextColor.svelte";

    var width = 1000,
        height = 1000;

    onMount(network);

    function network() {
        let links = $forceData.links;
        let nodes = $forceData.nodes;

        let svg = d3.select("svg").attr("width", width).attr("height", height);
        let simulation = d3
            .forceSimulation(nodes)
            .force("charge", d3.forceManyBody().strength(-250))
            .force("center", d3.forceCenter(width / 2, height / 2))
            .force(
                "link",
                d3.forceLink(links).id((d) => d.id)
            );

        let linkElements = svg
            .append("g")
            .selectAll("line")
            .data(links)
            .enter()
            .append("line")
            .attr("stroke-width", 1)
            .attr("stroke", "#E5E5E5");
        let nodeElements = svg
            .append("g")
            .selectAll("circle")
            .data(nodes)
            .enter()
            .append("circle")
            .attr("r", 10)
            .attr("fill", getNodeColor);

        let textElements = svg
            .append("g")
            .selectAll("text")
            .data(nodes)
            .enter()
            .append("text")
            .text((node) => node.label)
            .attr("font-size", 15)
            .attr("dx", 15)
            .attr("dy", 4);

        simulation.on("tick", () => {
            nodeElements
                .attr("cx", (node) => node.x)
                .attr("cy", (node) => node.y);
            textElements
                .attr("x", (node) => node.x)
                .attr("y", (node) => node.y);
            linkElements
                .attr("x1", (link) => link.source.x)
                .attr("y1", (link) => link.source.y)
                .attr("x2", (link) => link.target.x)
                .attr("y2", (link) => link.target.y);
        });

        const dragDrop = d3
            .drag()
            .on("start", (e, node) => {
                node.fx = node.x;
                node.fy = node.y;
            })
            .on("drag", (e, node) => {
                simulation.alphaTarget(1.7).restart();
                node.fx = e.x;
                node.fy = e.y;
            })
            .on("end", (e, node) => {
                if (!e.active) {
                    simulation.alphaTarget(0);
                }
                node.fx = null;
                node.fy = null;
            });
        nodeElements.call(dragDrop);

        function selectNode(e, selectedNode) {
            const neighbors = getNeighbors(selectedNode, links);
            nodeElements.attr("fill", (node) => getNodeColor(node, neighbors));
            textElements.attr("fill", (node) => getTextColor(node, neighbors));
            linkElements.attr("stroke", (link) =>
                getLinkColor(selectedNode, link)
            );
        }
        nodeElements.on("click", selectNode);
    }
</script>

<svg class="force" />
