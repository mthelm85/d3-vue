<template>
  <v-app>
    <v-content>
      <TidyTree />
    </v-content>
  </v-app>
</template>

<script>
import TidyTree from '@/components/TidyTree'
import SimpleTree from '@/components/SimpleTree'

export default {
  name: 'App',
  components: {
    SimpleTree,
    TidyTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
