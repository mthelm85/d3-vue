<template lang="html">
  <svg :width="width" :height="height" style="{ boxSizing: borderBox }">
    <g class="link" :transform="treeTransform">
      <g>
        <path
          v-for="d in links"
          :d="linkRadial(d)"
          >
        </path>
      </g>
      <g>
        <circle
          v-for="d in descendants"
          :key="d.id"
          :class="circleClass(d)"
          :r="nodeSize(d)"
          :transform="`rotate(${d.x * 180 / Math.PI - 90}) translate(${d.y}, 0)`">
        </circle>
      </g>
      <g>
        <text
          v-for="(d) in descendants"
          :class="textClass(d)"
          :key="d.id"
          :text-anchor="textAnchor(d)"
          dy="0.35em"
          x="10"
          >
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
      height: 1000,
      width: 1000,
      links: null,
      descendants: null,
      descendantsSliced: null,
      root: null
    }
  },

  computed: {
    linkRadial () {
      return d3.linkRadial()
        .angle(d => d.x)
        .radius(d => d.y)
    },
    radius () {
      return this.width / 2
    },
    tree () {
      return d3.tree()
        .size([2 * Math.PI, this.radius])
        .separation((a, b) => (a.parent === b.parent ? 1 : 2) / a.depth)
    },
    treeTransform () {
      return `translate(${this.width / 2}, ${this.width / 2})`
    }
  },

  methods: {
    circleClass (d) {
      return d.children ? 'node--internal' : 'node--leaf'
    },
    nodeSize (d) {
      switch (d.depth) {
        case 0: return '5.5'
        case 1: return '4.5'
        case 2: return '3.5'
        case 3: return '2.5'
        case 4: return '1.5'
        case 5: return '0.5'
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
    },
    textAnchor (d) {
      return (d.x < Math.PI) === !d.children ? 'start' : 'end'
    },
    textTransform (d) {
      return d.x >= Math.PI ? 'rotate(180)' : null
    },
    textX (d) {
      return (d.x < Math.PI) === !d.children ? 6 : -6
    }
  },

  mounted () {
    this.root = d3.hierarchy(this.data)
    this.descendants = this.root.descendants()
    this.links = this.root.links()
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
  stroke-opacity: 0.3;
  stroke-width: 1px;
}

.text1 {
  font: 12px sans-serif;
}

.text2 {
  font: 10px sans-serif;
}

.text3 {
  font: 8px sans-serif;
}

.text4 {
  font: 6px sans-serif;
}

.text5 {
  font: 5px sans-serif;
}

.text6 {
  font: 4px sans-serif;
}
</style>
