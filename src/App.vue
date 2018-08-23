<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer/>
      <FileSelector @fileSelected="onFileSelected"/>
      <v-btn icon @click.stop="rightDrawer = !rightDrawer">
        <v-icon>more_vert</v-icon>
      </v-btn>
    </v-toolbar>

    <v-content>
      <v-container fluid>
        <v-layout align-start justify-center row wrap>
          <div v-for="(data, index) in graphDatas" :key="index">
            <chart-card :graphData="data" :index="index" @delData="graphDatas.splice(index, 1)" @apply="applySettings" />
          </div>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import FileSelector from './components/FileSelector'
import ChartCard from './components/ChartCard'
import Papa from 'papaparse'

export default {
  name: 'App',
  components: {
    FileSelector,
    ChartCard,
    Papa
  },
  data () {
    return {
      rightDrawer: false,
      title: 'GraphPlotter',
      colorPalette: [
        "rgba( 77, 196, 255, 0.2)",
        "rgba(  0,  90, 255, 0.2)",
        "rgba(  3, 175, 122, 0.2)",
        "rgba(255,  75,   0, 0.2)",
        "rgba(153,   0, 153, 0.2)",
        "rgba(255, 241,   0, 0.2)",
      ],
      colorPaletteBorder: [
        "rgba( 77, 196, 255, 0.5)",
        "rgba(  0,  90, 255, 0.5)",
        "rgba(  3, 175, 122, 0.5)",
        "rgba(255,  75,   0, 0.5)",
        "rgba(153,   0, 153, 0.5)",
        "rgba(255, 241,   0, 0.5)",
      ],
      graphDatas: [
        // {
        //   filename: "graph.csv",
        //   res_data: [
        //     ["id", "sample"],
        //     ["d1", 300],
        //     ["d2", 400],
        //     ["d3", 150]
        //   ],
        //   type: 'line',
        //   xAxis: 0,
        //   yAxis: [1],
        //   datacollection: {
        //     labels: ["d1", "d2", "d3"],
        //     datasets: [{
        //       label: "sample",
        //       data: [300, 400, 150]
        //     }]
        //   },
        //   options: {}
        // },
        // {
        //   filename: "graph.csv",
        //   res_data: [
        //     ["id", "sample"],
        //     [100, 300],
        //     [200, 400],
        //     [300, 150]
        //   ],
        //   type: 'scatter',
        //   xAxis: 0,
        //   yAxis: [1],
        //   datacollection: {
        //     datasets: [{
        //       label: "sample",
        //       data: [
        //         { x: 100, y: 300 },
        //         { x: 200, y: 400 },
        //         { x: 300, y: 150 }
        //       ]
        //     }]
        //   },
        //   options: {
        //     showLines: true
        //   }
        // }
      ]
    }
  },
  methods: {
    onFileSelected(files) {
      var file = files[0];
      let that = this;
      Papa.parse(file, {
        header: false,
        dynamicTyping: true,
        complete(res) {
          let info = that.csv2info(file.name, res.data, 'scatter', 0, [1]);
          that.graphDatas.push(info);
        }
      });
    },
    csv2info(filename, res_data, type, xAxis=0, yAxis=[1], options={}) {
      const transpose = a => a[0].map((_, c) => a.map(r => r[c]));
      let data = transpose(res_data);
      let info = {
        filename: filename,
        res_data: res_data,
        type: type,
        xAxis: xAxis,
        yAxis: yAxis,
        datacollection: {
          labels: [],
          datasets: []
        },
        options: options
      };
      // let cols = data.length;
      if(type === 'line') {
        info.datacollection.labels = data[xAxis].slice(1);
        for(let i of yAxis) {
          info.datacollection.datasets.push({
            label: data[i][0],
            data: data[i].slice(1),
            fill: false,
            backgroundColor: this.colorPalette[i%6],
            borderColor: this.colorPaletteBorder[i%6]
          });
        }
        return info;
      } else if(type === 'scatter') {
        for(let y of yAxis) {
          let poss = [];
          for(let i = 1; i < data[y].length; ++i) {
            poss.push({x: data[xAxis][i], y: data[y][i]});
          }
          info.datacollection.datasets.push({
            label: data[y][0],
            data: poss,
            fill: false,
            backgroundColor: this.colorPalette[y%6],
            borderColor: this.colorPaletteBorder[y%6]
          });
        }
        return info;
      }
    },
    applySettings(newInfo, index) {
      let info = this.csv2info(newInfo.filename, newInfo.res_data, newInfo.type, newInfo.xAxis, newInfo.yAxis, newInfo.options);
      this.graphDatas[parseInt(index)] = info;
      this.$forceUpdate();
    }
  }
}
</script>
