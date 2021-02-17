<script context="module">
    export function getTextColor(node, neighbors) {
        return neighbors.indexOf(node.id) ? "green" : "black";
    }
</script>
