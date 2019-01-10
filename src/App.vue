<template>
  <v-app>
    <v-content>
      <TidyTree />
    </v-content>
  </v-app>
</template>

<script>
import TidyTree from '@/components/TidyTree'

export default {
  name: 'App',
  components: {
    TidyTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
