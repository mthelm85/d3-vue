<template lang="html">
  <svg :width="width" :height="height">
    <g transform="translate(10, 10)">
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
          :r="nodeSize(d)"
          :transform="`translate(${d.y}, ${d.x})`">
        </circle>
      </g>
      <g>
        <text
          :class="textClass(d)"
          v-for="(d) in descendants"
          :key="d.id"
          :transform="`translate(${d.y + 6}, ${d.x + 4.5})`">
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
      data: require('@/assets/naicsTreeFull.json'),
      height: 10000,
      width: 1600,
      links: null,
      descendants: null,
      descendantsSliced: null,
      root: null
    }
  },

  computed: {
    tree () {
      return d3.tree()
        .size([this.height - 100, this.width - 200])
        .separation((a, b) => a.parent === b.parent ? 1 : 1.5)
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
    nodeSize (d) {
      switch (d.depth) {
        case 0: return '6.5'
        case 1: return '5.5'
        case 2: return '4.5'
        case 3: return '3.5'
        case 4: return '2.5'
        case 5: return '1.5'
      }
    },
    nodeText (d) {
      return d.data.name.length > 35 ? `${d.data.name.substring(0, 35)}...` : d.data.name
    },
    textClass (d) {
      switch (d.depth) {
        case 0: return 'text1'
        case 1: return 'text2'
        case 2: return 'text3'
        case 3: return 'text4'
        case 4: return 'text5'
        case 5: return 'text6'
      }
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

<style lang="css">
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

.text1 {
  font: 16px sans-serif;
}

.text2 {
  font: 14px sans-serif;
}

.text3 {
  font: 13px sans-serif;
}

.text4 {
  font: 12px sans-serif;
}

.text5 {
  font: 11px sans-serif;
}

.text6 {
  font: 10px sans-serif;
}
</style>
