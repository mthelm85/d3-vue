<template lang="html">
  <svg :width="width" :height="height">
    <path
      v-for="f in data.features"
      :d="path(f)"></path>
  </svg>
</template>

<script>
import * as d3 from 'd3'
export default {
  data () {
    return {
      data: require('@/assets/DO_COUNTIES_GEO.json'),
      width: 600,
      height: 600,
      path: null,
      projection: null
    }
  },

  mounted () {
    this.projection = d3.geoEquirectangular()
      .scale(150)

    this.path = d3.geoPath()
      .projection(this.projection)

    this.path(this.data)
  }
}
</script>

<style lang="css">
</style>
