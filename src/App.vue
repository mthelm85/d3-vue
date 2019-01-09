<template>
  <v-app>
    <v-content>
      <SimpleTree/>
    </v-content>
  </v-app>
</template>

<script>

import SimpleTree from '@/components/SimpleTree'

export default {
  name: 'App',
  components: {
    SimpleTree
  },
  data () {
    return {
      //
    }
  }
}
</script>
