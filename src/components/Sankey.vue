<template lang="html">
  <svg :width="width" :height="height">
    <g transform="translate(10 5)">
      <g>
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
          :key="l.value + l.y0"
          stroke="#606060"
          :stroke-width="Math.max(1, l.width)"
          stroke-opacity="0.4"
          >
        </path>
        <linearGradient
          v-for="l in data.links"
          :id="l.target.name + l.source.name"
          :key="l.target.name + l.source.name"
          gradientUnits="userSpaceOnUse"
          :x1="l.source.x1"
          :x2="l.target.x0">
          <stop offset="0%" :stop-color="color(l.source.name.replace(/ .*/, ''))"></stop>
          <stop offset="100%" :stop-color="color(l.target.name.replace(/ .*/, ''))"></stop>
        </linearGradient>
      </g>
      <g>
        <text
          v-for="n in data.nodes"
          :x="n.x0 < width / 2 ? n.x1 + 6 : n.x0 - 6"
          :y="(n.y1 + n.y0) / 2"
          text-anchor="n.x0 < width / 2 ? 'start : 'end"
          transform="translate(-20 5)">
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
      data: require('@/assets/cmp.json'),
      color: null,
      sankey: null,
      sankeyLink: null,
      width: 1400,
      height: 1800
    }
  },

  methods: {
    rectHeight (y1, y0) {
      return Math.abs(y1 - y0)
    },
    rectWidth (x1, x0) {
      return Math.abs(x1 - x0)
    }
  },

  mounted () {
    this.sankey = Sankey.sankey()
      .nodeWidth(25)
      .nodePadding(10)
      .size([this.width, this.height])

    this.color = d3.scaleOrdinal(d3.schemeCategory10)
    this.sankey(this.data)
    this.sankeyLink = Sankey.sankeyLinkHorizontal()
  }
}
</script>

<style lang="css">
</style>
