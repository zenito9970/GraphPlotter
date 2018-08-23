<template>
  <v-flex>
    <v-card class="ma-2 pa-3">
      <chart :datacollection="this.graphData.datacollection" :options="this.graphData.options" :chartType="this.graphData.type" />
      <v-card-actions>
        <span>{{ this.graphData.filename }}</span>
        <v-spacer/>
        <v-btn icon @click.stop="reloadBtn"><v-icon>cached</v-icon></v-btn>
        <v-btn icon><v-icon>save</v-icon></v-btn>
        <GraphSettingsDialog :info="graphData" @apply="applySettings" />
        <v-btn icon @click.stop="deleteBtn"><v-icon>clear</v-icon></v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
</template>


<script>
import Chart from './Chart'
import GraphSettingsDialog from './GraphSettingsDialog'

export default {
  name: 'ChartCard',
  components: {
    Chart,
    GraphSettingsDialog
  },
  props: ['graphData', 'index'],
  methods: {
    deleteBtn() {
      this.$emit('delData');
    },
    reloadBtn() {
      this.$emit('reloadData');
    },
    applySettings(newInfo) {
      this.$emit('apply', newInfo, this.index);
    }
  }
}
</script>

