<template>
  <div id="app">
    Daily
    <bar-chart :chart-data="dailyData" :height="170" />
    <br />
    Accumulated
    <line-chart :chart-data="accData" :height="170" />
  </div>
</template>

<script>
import LineChart from "./components/LineChart.vue";
import BarChart from "./components/BarChart.vue";
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      dailyData: {
        labels: [],
        datasets: [],
      },
      accData: {
        labels: [],
        datasets: [],
      },
    };
  },
  components: {
    LineChart,
    BarChart,
  },
  created() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      const response = await axios({
        method: "GET",
        url:
          "https://raw.githubusercontent.com/jiyeonseo/ring-fit-adventure-exercise-log/master/ringfit.csv",
      })
        .then(function(response) {
          return response;
        })
        .catch(function(error) {
          console.log(error);
        });
      const raw = response.data.split("\n");

      const date = [];
      const time = [];
      const cal = [];
      const distance = [];

      for (var idx in raw) {
        const daily = raw[idx].split(",");
        date.push(daily[0]);
        time.push((parseInt(daily[1]) || 0) / 60);
        cal.push(parseFloat(daily[2]) || 0);
        distance.push(parseFloat(daily[3]) || 0);
      }

      const acctime = time.map(((acc) => (val) => (acc += val))(0));
      const acccal = cal.map(((acc) => (val) => (acc += val))(0));
      const accdistance = distance.map(((acc) => (val) => (acc += val))(0));

      this.dailyData = {
        labels: date,
        datasets: [
          {
            label: "time(mins)",
            backgroundColor: "#f87979",
            data: time,
          },
          {
            label: "cal",
            backgroundColor: "#ffcd56",
            data: cal,
          },
          {
            label: "distance(km)",
            backgroundColor: "#36a2eb",
            data: distance,
          },
        ],
      };
      this.accData = {
        labels: date,
        datasets: [
          {
            label: "time(mins)",
            backgroundColor: "#f87979",
            data: acctime,
          },
          {
            label: "cal",
            backgroundColor: "#ffcd56",
            data: acccal,
          },
          {
            label: "distance(km)",
            backgroundColor: "#36a2eb",
            data: accdistance,
          },
        ],
      };
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
