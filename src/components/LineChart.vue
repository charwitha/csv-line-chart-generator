<script>
import { Line, mixins } from 'vue-chartjs'
 
export default {
  extends: Line,
  mixins: [mixins.reactiveProp],
  props: ['chartData', 'options'],
  watch: {
    'chartData': {
      handler(newValue, oldValue) {
        this._data._chart.destroy()
        this.renderChart(this.chartData, this.options)
      },
      deep: true
    },
    'options': {
      handler(newValue, oldValue) {
        this._data._chart.destroy()
        this.renderChart(this.chartData, this.options)
      },
      deep: true
    }
  },
  mounted () {
    this.renderChart(this.chartData, this.options)
  }
}
</script>