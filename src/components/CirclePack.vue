<template lang="html">
  <svg :width="width" :height="height" transform="translate(10, 10)">
    <text
      x="50%"
      transform="translate(0, 20)"
      text-anchor="middle"
      class="circle-label">
      2018 BWs by DO
    </text>
    <g opacity="0.85" transform="translate(0, 10)">
      <circle
        v-for="d in descendants"
        :fill="color(d.data.name.replace(/ .*/, ''))"
        :cx="d.x"
        :cy="d.y"
        :r="d.r"
        stroke="black">
        <title>
          <text>{{ d.data.name }}: ${{ d.value.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) }}</text>
        </title>
      </circle>
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
      height: 960
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
.circle-label {
  font: 22px sans-serif;
}
</style>
