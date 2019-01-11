<template>
  <v-app>
    <v-content>
      <Sankey23 />
    </v-content>
  </v-app>
</template>

<script>
import Sankey from '@/components/Sankey'
import Sankey23 from '@/components/Sankey23'
import TidyTree from '@/components/TidyTree'

export default {
  name: 'App',
  components: {
    Sankey,
    Sankey23,
    TidyTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
