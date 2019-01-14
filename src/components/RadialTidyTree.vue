<template lang="html">
  <svg :width="width" :height="height">
    <g :transform="treeTransform" >
      <g class="link">
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
          :transform="textTransform(d)"
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
      data: require('@/assets/bwdo.json'),
      height: 900,
      width: 900,
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
        .size([2 * Math.PI, this.radius * .7])
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
        case 0: return 'radial-text1'
        case 1: return 'radial-text2'
        case 2: return 'radial-text3'
        case 3: return 'radial-text4'
        case 4: return 'radial-text5'
        case 5: return 'radial-text6'
      }
    },
    textAnchor (d) {
      return d.x < Math.PI ? 'start' : 'end'
    },
    textTransform (d) {
      return d.x < Math.PI ? `rotate(${d.x * 180 / Math.PI - 90}) translate(${d.y + 8}, 0)` : `rotate(${d.x * 180 / Math.PI - 90}) translate(${d.y + 8}, 0) scale(-1, -1)`
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

.radial-text1 {
  font: 16px sans-serif;
}

.radial-text2 {
  font: 14px sans-serif;
}

.radial-text3 {
  font: 12px sans-serif;
}

.radial-text4 {
  font: 6px sans-serif;
}

.radial-text5 {
  font: 5px sans-serif;
}

.radial-text6 {
  font: 4px sans-serif;
}
</style>
