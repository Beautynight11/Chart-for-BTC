<template>
  <Line
      :chart-options="chartOptions"
      :chart-data="chartData"
      :chart-id="chartId"
      :dataset-id-key="datasetIdKey"
      :plugins="plugins"
      :css-classes="cssClasses"
      :styles="styles"
      :width="width"
      :height="height"
  />
  <div>{{this.time}}</div>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale
} from 'chart.js'

ChartJS.register(
    Title,
    Tooltip,
    Legend,
    LineElement,
    LinearScale,
    PointElement,
    CategoryScale
)

export default {
  name: 'LineChart',
  components: { Line },
  props: {
    chartId: {
      type: String,
      default: 'line-chart'
    },
    datasetIdKey: {
      type: String,
      default: 'label'
    },
    width: {
      type: Number,
      default: 400
    },
    height: {
      type: Number,
      default: 200
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object,
      default: () => {}
    },
    plugins: {
      type: Object,
      default: () => {}
    },

  },
  data() {
    return {
      chartData: {
        labels: [],
        datasets: [
          {
            label: 'Data One',
            backgroundColor: '#f87979',
            data: [40, 39, 10, 40, 39, 80, 40],
            borderColor: '#f87979'
          },
          {
            label: 'Data two',
            backgroundColor: '#5695f6',
            data: [1, 2, 3],
            borderColor: '#5695f6'
          },
          {
            label: 'Data three',
            backgroundColor: '#56f6b3',
            data: [4, 5, 6],
            borderColor: '#56f6b3'
          },
        ]
      },
      chartOptions: {
        responsive: true
      },
      time: null,
      interval: '',
    }
  },
  methods: {
    getTime() {
      this.time = new Date().toLocaleTimeString();
    },
    stopTimer () {
      if (this.interval) {
        window.clearInterval(this.interval);
      }
    },
    startTimer () {
      this.stopTimer();
      this.interval = window.setInterval(() => {
        this.getTime();
      }, 2000)
    },
  },
  mounted() {
    this.getTime();
    this.startTimer()
  },
  beforeUnmount() {
    this.stopTimer()
  },
}
</script>