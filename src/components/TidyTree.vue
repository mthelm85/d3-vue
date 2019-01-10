<template lang="html">
  <svg :width="width" :height="height">
    <g transform="translate(40, 0)">
      <g>
        <path
          v-for="d in descendantsSliced"
          class="link"
          :d="diagonal(d)">
        </path>
      </g>
      <g>
        <circle
          v-for="d in descendants"
          :class="circleClass(d)"
          r="2.5"
          :transform="`translate(${d.y}, ${d.x})`">
        </circle>
      </g>
      <g>
        <text
          class="text"
          v-for="(d, i) in descendants"
          :transform="`translate(${d.y + 5}, ${d.x + 2.5})`">
          {{ d.data.id }}
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
      height: 7000,
      width: 1600,
      links: null,
      descendants: null,
      descendantsSliced: null,
      root: null,
      tree: null
    }
  },

  methods: {
    circleClass (d) {
      return d.children ? 'node--internal' : 'node--leaf'
    },
    diagonal (d) {
      return `
        M${d.y}, ${d.x}
        C${d.parent.y + 100}, ${d.x}
         ${d.parent.y + 100}, ${d.parent.x}
         ${d.parent.y}, ${d.parent.x}
      `
    }
  },

  mounted () {
    this.root = d3.hierarchy(this.data)
    this.descendants = this.root.descendants()
    this.links = this.root.links()
    this.tree = d3.tree()
      .size([this.height - 100, this.width - 200])
      .separation((a, b) => a.parent === b.parent ? 1 : 1.5)
    this.tree(this.root)
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
  stroke-opacity: 0.4;
  stroke-width: 1.5px;
}

.text {
  font: 10px sans-serif;
}
</style>
