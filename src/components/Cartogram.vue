<template lang="html">
  <svg :width="width" :height="height">
    <path
      v-for="f in data.features"
      :d="path(f)"
      class="land">
    </path>
    <!-- <path
      v-for="f in data.features"
      class="state"
      :d="path(f)"
      :transform="pathTransform(f)"
      >
    </path> -->
  </svg>
</template>

<script>
import * as d3 from 'd3'
export default {
  data () {
    return {
      data: require('@/assets/DO_COUNTIES_GEO.json'),
      width: 900,
      height: 600,
      path: null,
      projection: null,
      transition: null
    }
  },

  created () {
    this.projection = d3.geoAlbersUsa()
      .scale(900)

    this.path = d3.geoPath()
      .projection(this.projection)

    this.path(this.data)
  },

  methods: {
    pathTransform (f) {
      let centroid = this.path.centroid(f),
          x = centroid[0],
          y = centroid[1]
      return `translate(${x}, ${y}) scale(${Math.sqrt(f.properties.DO_ID / 1.1 * .0001) * 5 || 0}) translate(${-x}, ${-y})`
    }
  }
}
</script>

<style lang="css">
.land {
  fill: #fff;
  stroke: #ccc;
}

.land:hover {
  fill: #ccc;
}

.state {
  fill: #ccc;
  stroke: #666;
}
</style>
