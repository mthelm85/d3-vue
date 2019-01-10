<template lang="html">
  <svg :width="width" :height="height" transform="translate(20 0)">
    <g id="nodes-links" transform="translate(20 20)">
      <g class="links">
        <!-- <line
          class="link"
          v-for="l in links"
          :key="l.id"
          :x1="l.source.x"
          :y1="l.source.y"
          :x2="l.target.x"
          :y2="l.target.y" /> -->
        <path
          class="link"
          v-for="l in links"
          :key="l.id"
          :d="computePath(l)">
        </path>
      </g>
      <g class="nodes">
          <circle
            class="node"
            v-for="d in descendants"
            :key="d.id"
            :cx="d.x"
            :cy="d.y"
            r=4></circle>
      </g>
      <g>
        <text
          class="text"
          v-for="(d, i) in descendants"
          :key="d+i"
          :dx="d.x"
          :dy="d.y"
          transform="translate(10 5)">
          {{ d.data.name }}
        </text>
      </g>
    </g>
  </svg>
</template>

<script>
import * as d3 from 'd3'

export default {
  data () {
    return {
      data: require('@/assets/naicsTree.json'),
      descendants: null,
      height: 3000,
      links: null,
      root: null,
      treeLayout: null,
      width: 5000

    }
  },

  methods: {
    computePath (l) {
      // see https://www.dashingd3js.com/svg-paths-and-d3js and d3 cluster dendrogram on observable
      return `
        M${l.source.x},${l.source.y}
        S${l.source.x},${l.source.y}
        ${l.target.x},${l.target.y}
      `
    }
  },

  mounted () {
    // convert our json to a d3 hierarchy object
    this.root = d3.hierarchy(this.data)

    // get an array of all of the nodes
    this.descendants = this.root.descendants()

    // get an array of all of the links
    this.links = this.root.links()

    // Create the tree layout, set up size, add some padding
    this.treeLayout = d3.tree()
      .size([3000, 2000])

    // pass root to treeLayout which writes x, y values on each node of root (used for positioning)
    this.treeLayout(this.root)

    // move nodes/links down and to the left
    d3.select('#nodes-links')
      .attr('transform', 'translate(20 20)')
  }
}
</script>

<style lang="css">
.node {
  fill: steelblue;
  stroke: none;
}

.link {
  fill: none;
  stroke: #ccc;
  stroke-width: 1px;
}

</style>
