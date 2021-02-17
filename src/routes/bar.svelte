<script>
    import { barData } from "../store.js";
    import * as d3 from "d3";
    import { onMount } from "svelte";

    let svgWidth = 500,
        svgHeight = 500,
        barPadding = 10;
    let barWidth = svgWidth / $barData.length;
    onMount(() => {
        let svg = d3
            .select("svg")
            .attr("width", svgWidth)
            .attr("height", svgHeight);
        let barChart = svg
            .selectAll("rect")
            .data($barData)
            .enter()
            .append("rect")
            .attr("y", function (d) {
                return svgHeight - d;
            })
            .attr("height", function (d) {
                return d;
            })
            .attr("width", barWidth - barPadding)
            .attr("transform", function (d, i) {
                var translate = [barWidth * i, 0];
                return "translate(" + translate + ")";
            });
    });
</script>

<style>
</style>

<svelte:head>
    <title>Bar Chart</title>
</svelte:head>

<svg class="bar-chart" />
