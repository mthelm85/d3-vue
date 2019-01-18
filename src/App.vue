<template>
  <v-app>
    <v-content>
      <Cartogram />
    </v-content>
  </v-app>
</template>

<script>
import Calendar from '@/components/Calendar'
import CalendarBlurred from '@/components/CalendarBlurred'
import Cartogram from '@/components/Cartogram'
import Sankey from '@/components/Sankey'
import Sankey23 from '@/components/Sankey23'
import TidyTree from '@/components/TidyTree'
import RadialTidyTree from '@/components/RadialTidyTree'
import CirclePack from '@/components/CirclePack'

export default {
  name: 'App',
  components: {
    Calendar,
    CalendarBlurred,
    Cartogram,
    CirclePack,
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
