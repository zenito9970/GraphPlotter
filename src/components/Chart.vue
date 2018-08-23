<template>
  <div>
    <v-layout justify-center row>
      <LineChart v-if="this.chartType==='line'" :chartData="this.datacollection" :options="this.options" />
      <ScatterChart v-else :chartData="this.datacollection" :options="this.options" />
    </v-layout>
  </div>
</template>


<script>
import LineChart from "./charts/LineChart";
import ScatterChart from "./charts/ScatterChart";

export default {
  name: "Chart",
  components: {
    LineChart,
    ScatterChart
  },
  props: ['datacollection', 'options', 'chartType']
}
</script>
