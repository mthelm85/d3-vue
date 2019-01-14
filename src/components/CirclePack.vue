<template lang="html">
  <svg :width="width" :height="height" transform="translate(10, 10)">
    <g opacity="0.85">
      <circle
        v-for="d in descendants"
        :fill="color(d.data.name.replace(/ .*/, ''))"
        :cx="d.x"
        :cy="d.y"
        :r="d.r">
        <title>
          <text>{{ d.data.name }}: ${{ d.value.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) }}</text>
        </title>
      </circle>
      <text
        v-for="d in descendants"
        :transform="`translate(${d.x}, ${d.y})`"
        dy="4">
        {{ d.data.name }}
      </text>
    </g>
  </svg>
</template>

<script>
import * as d3 from 'd3'

export default {
  data () {
    return {
      data: require('@/assets/bwdo.json'),
      descendants: null,
      root: null,
      width: 900,
      height: 900
    }
  },

  computed: {
    color () {
      return d3.scaleOrdinal(d3.schemeCategory10)
    },
    pack () {
      return d3.pack()
        .size([this.width, this.height])
        .padding(3)
    }
  },

  mounted () {
    this.root = d3.hierarchy(this.data)
      .sum(d => d.value)
    this.descendants = this.root.descendants()
    this.pack(this.root)
  }

}
</script>

<style lang="css">
</style>
