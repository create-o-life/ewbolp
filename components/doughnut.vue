<script>
import { Doughnut } from 'vue-chartjs'
export default {
  extends: Doughnut,
  props:['data'],
  mounted () {
    this.renderChart(this.data.chartData, this.data.options)
  }
}
</script>