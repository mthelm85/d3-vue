<template lang="html">
  <svg :width="width" :height="height" transform="translate(10 10)">
    <g>
      <g stroke="#000">
        <rect
          class="node"
          v-for="n in data.nodes"
          :x="n.x0"
          :y="n.y0"
          :key="n.name"
          :height="n.y1- n.y0"
          :width="n.x1 - n.x0"
          :fill="color(n.name.replace(/ .*/, ''))">
        </rect>
      </g>
      <g fill="none" stroke-opacity="0.5">
        <path
          v-for="l in data.links"
          :d="sankeyLink(l)"
          :key="l.target.name + l.source.name"
          :stroke="`url(#${l.index})`"
          :stroke-width="Math.max(1, l.width)"
          class="sankey-link">
          <title>
            <text>${{ l.value.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) }}</text>
          </title>
        </path>
        <linearGradient
          v-for="l in data.links"
          :id="l.index"
          :key="l.source.name + l.target.name">
          <stop offset="0%" :stop-color="color(l.source.name.replace(/ .*/, ''))"></stop>
          <stop offset="100%" :stop-color="color(l.target.name.replace(/ .*/, ''))"></stop>
        </linearGradient>
      </g>
      <g>
        <text
          v-for="n in data.nodes"
          :class="nodeTextClass(n)"
          :x="n.x0 < width / 2 ? n.x1 + 6 : n.x0 - 6"
          :key="`${n.name}asd`"
          :y="(n.y1 + n.y0) / 2"
          :text-anchor="n.x0 < width / 2 ? 'start' : 'end'"
          :transform="textTransform(n.index)">
          {{ n.name }}
        </text>
      </g>
    </g>
  </svg>
</template>

<script>
import * as d3 from 'd3'
import * as Sankey from 'd3-sankey'
export default {
  data () {
    return {
      data: require('@/assets/cmpNames.json'), // $20k or more, excludes 09 naics
      width: 1200,
      height: 1040
    }
  },

  computed: {
    color () {
      return d3.scaleOrdinal(d3.schemeCategory10)
    },
    sankey () {
      return Sankey.sankey()
        .nodeWidth(25)
        .nodePadding(10)
        .size([this.width - 20, this.height - 20])
    },
    sankeyLink () {
      return Sankey.sankeyLinkHorizontal()
    }
  },

  methods: {
    nodeTextClass (n) {
      switch (n.depth) {
        case 0: return 'root-label'
        case 1: return 'mid-label'
        case 2: return 'node-label'
      }
    },
    rectHeight (y1, y0) {
      return Math.abs(y1 - y0)
    },
    rectWidth (x1, x0) {
      return Math.abs(x1 - x0)
    },
    textTransform (n) {
      return n === 0 ? 'translate(2, 5)' : 'translate(-2 5)'
    }
  },

  mounted () {

    this.sankey(this.data)
  }
}
</script>

<style lang="css">
.node-label {
  font: 10px sans-serif;
}

.root-label {
  font: 18px sans-serif;
}

.mid-label {
  font: 13px sans-serif;
}

.sankey-link {
  stroke-opacity: 0.4
}

.sankey-link:hover {
  stroke-opacity: 0.8
}
</style>
