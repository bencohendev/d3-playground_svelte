<script context="module">
    export function getLinkColor(node, link) {
        return isNeighborLink(node, link) ? "green" : "#E5E5E5";
    }
    function isNeighborLink(node, link) {
        return link.target.id === node.id || link.source.id === node.id;
    }
</script>
