<template>
    <div>
        <TreeSubMenu />

        <div class="content-section introduction">
            <div class="feature-intro">
                <h1>Tree - Filter</h1>
                <p>Filtering updates the node based on the constraints.</p>
            </div>
        </div>

        <div class="content-section implementation">
            <h3>Lenient Filter</h3>
            <Tree :value="nodes" :filter="true" filterMode="lenient"></Tree>

            <h3>Strict Filter</h3>
            <Tree :value="nodes" :filter="true" filterMode="strict"></Tree>          
        </div>

        <TreeDoc />
    </div>
</template>
<script>
import NodeService from '../../service/NodeService';
import TreeDoc from './TreeDoc';
import TreeSubMenu from './TreeSubMenu';

export default {
    data() {
        return {
            nodes: null,
            expandedKeys: {}
        }
    },
    nodeService: null,
    created() {
        this.nodeService = new NodeService();
    },
    mounted() {
        this.nodeService.getTreeNodes().then(data => this.nodes = data);
    },
    methods: {
        expandAll() {
            for (let node of this.nodes) {
                this.expandNode(node);
            }

            this.expandedKeys = {...this.expandedKeys};
        },
        collapseAll() {
            this.expandedKeys = {};
        },
        expandNode(node) {
            this.expandedKeys[node.key] = true;
            if (node.children && node.children.length) {
                for (let child of node.children) {
                    this.expandNode(child);
                }
            }
        }
    },
    components: {
        'TreeDoc': TreeDoc,
        'TreeSubMenu': TreeSubMenu
    }
}
</script>

<style scoped>
button {
    margin-right: .5em;
}
</style>