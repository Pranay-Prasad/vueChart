<template>
  <div class="hello">
    <!-- <ChartData tableData :="tableData" /> -->
    <!-- {{ tableData  }} -->
    <Bar
    id="my-chart-id"
    :options="chartOptions"
    :data="chartData"
  />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
export default ({
  name: 'HelloWorld',
  components: {
    Bar
  },
  props: {
    msg: String
  },
  data() {
    return {
      tableData: [],
      countperMonth: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      chartData: {
        datasets: [{ data: [1,2,3,0,0,0,5,6,7,15,9,1] }],
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      },
      chartOptions: {
        responsive: true
      }
    }
  },
  methods: {
    async getChart() {
      for (let index = 0; index < this.tableData.length; index++) {
        const element = this.tableData[index];
        this.countperMonth[parseInt(element.order_date.substr(5,2))]++;
      }
      this.chartData.datasets[0].data = this.countperMonth;
      console.log(this.chartData.datasets);
    },
    async fetchData() {
      console.log("RUN");
      await fetch("https://bee.nutshell.repair/api/v1/emp/reports/all_data?city=1&from_date=2023-01-01&to_date=2023-07-17&category[]=1&category[]=2&category[]=3&category[]=4&category[]=5&category[]=6&category[]=7&category[]=8&category[]=9", {
        method: 'GET',
        headers: {
          'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbXBfaWQiOjEwLCJlbXBfbW9iaWxlIjoiOTM3MzQ0ODAxNiIsInRva2VuX2NyZWF0ZWRfYXQiOjE2ODk1Nzk1OTMsInRva2VuX2V4cGlyZXNfYXQiOjE2OTEzMDc1OTN9.nW6EstOefPuhe7bD-mpIt2x4tzoga-RU5hXeUcThRis'
        }
      })
        .then(response => response.json())
        .then(data => {
          this.tableData = data.data;
          console.log(this.tableData);
          this.getChart();
        })
        .catch(error => {
          console.log(error);
        });

    }

  },
  async created() {
    await this.fetchData();
  },
  mounted(){
    this.fetchData(); 
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
