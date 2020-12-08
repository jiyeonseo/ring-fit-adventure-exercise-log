<script>
import { Line, mixins } from "vue-chartjs";
const { reactiveProp } = mixins;

export default {
  extends: Line,
  mixins: [reactiveProp],
  props: {
    options: {
      type: Object,
      default: () => {
        return {
          scales: {
            yAxes: [
              {
                ticks: {
                  suggestedMin: 0
                }
              }
            ]
          }
        };
      }
    }
  },
  mounted() {
    this.renderChart(this.chartData, this.options);
  }
};
</script>
