<template>
  <v-app>
    <v-content>
      <Sankey23 />
    </v-content>
  </v-app>
</template>

<script>
import Cartogram from '@/components/Cartogram'
import Sankey from '@/components/Sankey'
import Sankey23 from '@/components/Sankey23'
import TidyTree from '@/components/TidyTree'
import RadialTidyTree from '@/components/RadialTidyTree'

export default {
  name: 'App',
  components: {
    Cartogram,
    Sankey,
    Sankey23,
    TidyTree,
    RadialTidyTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
