<script context="module">
    export function getNodeColor(node, neighbors) {
        if (typeof neighbors === "object") {
            if (neighbors.indexOf(node.id)) {
                return node.level === 1 ? "blue" : "green";
            }
            return node.level === 1 ? "red" : "gray";
        }
    }
</script>
