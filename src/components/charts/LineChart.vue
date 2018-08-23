<script>
import { Line, mixins } from "vue-chartjs";

export default {
  extends: Line,
  mixins: [mixins.reactiveProp],
  props: ['chartData', 'options'],
  mounted() {
    this.renderThisChart();
  },
  methods: {
    renderThisChart() {
      let defaultOptions = {
        responsive: true,
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true
            }
          }]
        }
      };
      let opt = Object.assign(defaultOptions, this.options);
      this.renderChart(this.chartData, opt);
    }
  },
  watch: {
    chartData() {
      this.renderThisChart();
    },
    options() {
      this.renderThisChart();
    }
  }
}
</script>
