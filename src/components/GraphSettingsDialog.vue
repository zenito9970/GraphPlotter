<template>
  <div>
    <v-btn icon @click.stop="settingsBtn"><v-icon>settings</v-icon></v-btn>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <div>
            <div><strong>Settings</strong></div>
            <div><small>{{this.info.filename}}</small></div>
          </div>
        </v-card-title>
        <v-card-text>
          <p v-if="false">
            dialog: {{dialog}} <br/>
            yAxis: {{yAxis}} <br/>
            xAxis: {{xAxis}}
          </p>
          <v-layout row justify-start align-start>
            <div>
              <label>グラフタイプ</label>
              <v-radio-group v-model="type" style="margin-top:8px">
                <v-radio label="折れ線" value="line" />
                <v-radio label="散布図" value="scatter" />
              </v-radio-group>
            </div>
            <v-spacer/>
            <div>
              <label>X軸</label>
              <v-radio-group v-model="xAxis" style="margin-top:8px">
                <v-radio v-for="(head, index) in this.info.res_data[0]" :key="index" :label="head" :value="`${index}`" />
              </v-radio-group>
            </div>
            <v-spacer/>
            <div>
              <label>プロットデータ</label>
              <div style="margin-top:8px"></div>
              <v-checkbox v-for="(head, index) in this.info.res_data[0]" :key="index" v-model="yAxis" :label="head" :value="`${index}`" class="radio-like-chkbox" />
            </div>
            <v-spacer/>
          </v-layout>
          <div v-if="type === 'line'">
            <v-checkbox v-model="yBegin0" label="Y軸の原点を0にする" class="radio-like-chkbox" />
          </div>
          <div v-if="type === 'scatter'">
            <v-checkbox v-model="xBegin0" label="X軸の原点を0にする" class="radio-like-chkbox" />
            <v-checkbox v-model="yBegin0" label="Y軸の原点を0にする" class="radio-like-chkbox" />
          </div>
        </v-card-text>
        <v-card-actions>
          <v-spacer/>
          <v-btn color="primary" flat @click.stop="applyBtn">Apply</v-btn>
          <v-btn color="error" flat @click.stop="cancelBtn">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<style scoped>
.radio-like-chkbox {
  margin-top:0;
  margin-bottom:8px;
  height:24px
}
</style>



<script>
export default {
  name: "GraphSettingsDialog",
  props: ['info'],
  data() {
    return {
      dialog: false,
      yAxis: this.info.yAxis.map(a=>a.toString()),
      xAxis: this.info.xAxis.toString(),
      type: this.info.type,
      options: {},
      xBegin0: true,
      yBegin0: true
    }
  },
  methods: {
    settingsBtn() {
      this.dialog = true;
    },
    applyBtn() {
      this.dialog = false;
      this.setOptions();
      let retInfo = {
        filename: this.info.filename,
        res_data: this.info.res_data,
        type: this.type,
        xAxis: parseInt(this.xAxis),
        yAxis: this.yAxis.map(a=>parseInt(a)).sort((a, b) => { return a - b; }),
        datacollection: this.info.datacollection,
        options: this.options
      };
      this.$emit('apply', retInfo);
    },
    setOptions() {
      this.options = {
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: this.yBegin0
            }
          }],
          xAxes: [{
            ticks: {
              beginAtZero: this.xBegin0
            }
          }]
        }
      };
    },
    cancelBtn() {
      this.dialog = false;
    }
  }
}
</script>

