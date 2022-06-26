<template>
  <div>
    <Bar
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

  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  props: {
    chartId: {
      type: String,
      default: 'bar-chart'
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
      default: 400
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
    }
  },
  data() {
    return {
      chartData: {
        labels: [ 'MAX鈴木', 'ぞうさんパクパク', 'えびまよ', 'しのけん' ],
        datasets: [ { data: [0, 0, 0, 0] } ]
      },
      chartOptions: {
        responsive: true
      },
      apiKey: process.env.VUE_APP_API_KEY_2,
    }
  },
  async mounted() {
    for (const fighterIndex in this.chartData.labels) {
      console.log(this.chartData.labels[fighterIndex]);
      // TODO: URLオブジェクトのsetメソッドでパラメータ追加. パラメータはdataプロパティ化
      const videosData = await fetch('https://www.googleapis.com/youtube/v3/search?type=video&part=id,snippet&order=viewCount&q=intitle:大食い+intitle:'+this.chartData.labels[fighterIndex]+'&maxResults=5&key='+this.apiKey);
      let videos = await videosData.json();
      console.log(videos.items);
      for (let videoIndex in videos.items) {
        const id = videos.items[videoIndex].id.videoId;
        const videoData = await fetch('https://www.googleapis.com/youtube/v3/videos?part=statistics&id='+id+'&key='+this.apiKey);
        const video = await videoData.json();
        console.log(fighterIndex);
        console.log(this.chartData.datasets);
        console.log(this.chartData.datasets[0]);
        console.log(video.items[0].statistics.viewCount);
        // TODO: datasets[0], data[0]は0~4でループ
        this.chartData.datasets[0].data[fighterIndex] += parseInt(video.items[0].statistics.viewCount)
      }
      console.log(this.chartData)
    }
  }
}
</script>