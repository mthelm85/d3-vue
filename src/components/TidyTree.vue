<template lang="html">
  <svg :width="width" :height="height">
    <g transform="translate(70, 10)">
      <g>
        <path
          v-for="d in descendantsSliced"
          :key="d.id"
          class="link"
          :d="diagonal(d)">
        </path>
      </g>
      <g>
        <circle
          v-for="d in descendants"
          :key="d.id"
          :class="circleClass(d)"
          r="4.5"
          :transform="`translate(${d.y}, ${d.x})`">
        </circle>
      </g>
      <g>
        <text
          v-for="(d) in descendants"
          :key="d.id"
          :transform="textTransform(d)"
          :text-anchor="d.children ? 'end' : 'start'">
          {{ nodeText(d) }}
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
      data: require('@/assets/bwdo.json'),
      height: 900,
      width: 700,
      links: null,
      descendants: null,
      descendantsSliced: null,
      root: null
    }
  },

  computed: {
    // Create the tree layout, set up size, add some separation between cousin nodes
    tree () {
      return d3.tree()
        .size([this.height - 200, this.width - 250])
        // .separation((a, b) => a.parent === b.parent ? 1 : 1.5)
    }
  },

  methods: {
    circleClass (d) {
      return d.children ? 'node--internal' : 'node--leaf'
    },
    // see https://www.dashingd3js.com/svg-paths-and-d3js and d3 cluster dendrogram on observable
    diagonal (d) {
      return `
        M${d.y}, ${d.x}
        C${d.parent.y + 100}, ${d.x}
         ${d.parent.y + 100}, ${d.parent.x}
         ${d.parent.y}, ${d.parent.x}
      `
    },
    nodeText (d) {
      return d.data.name.length > 35 ? `${d.data.name.substring(0, 35)}...` : d.data.name
    },
    textTransform (d) {
      return d.children ? `translate(${d.y - 8}, ${d.x + 3.5})` : `translate(${d.y + 6}, ${d.x + 3.5})`
    }
  },

  mounted () {
    // convert our json to a d3 hierarchy object, store as root
    this.root = d3.hierarchy(this.data)
    // get an array of all of the nodes
    this.descendants = this.root.descendants()
    // get an array of all of the links
    this.links = this.root.links()
    // pass root to treeLayout which writes x, y values on each node of root (used for positioning)
    this.tree(this.root)
    // create a copy of descendants, without the parent node (this is for the paths)
    this.descendantsSliced = this.descendants.slice(1)
  }
}
</script>

<style scoped lang="css">
.node--internal {
  fill: #000;
}

.node--leaf {
  fill: #b2b2b2;
}

.link {
  fill: none;
  stroke: #555;
  stroke-opacity: 0.3;
  stroke-width: 1px;
}

text {
  font: 11px sans-serif;
}

text:hover {
  fill: #555;
  cursor: default;
}
</style>
