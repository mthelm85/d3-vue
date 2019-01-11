<template>
  <v-app>
    <v-content>
      <Sankey />
    </v-content>
  </v-app>
</template>

<script>
import Sankey from '@/components/Sankey'
import TidyTree from '@/components/TidyTree'

export default {
  name: 'App',
  components: {
    Sankey,
    TidyTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
