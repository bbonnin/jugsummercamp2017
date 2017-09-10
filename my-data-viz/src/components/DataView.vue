<template>
    <v-app id="demo1">
      <v-toolbar class="blue darken-2 white--text">
        <v-toolbar-title>A chart in 2 seconds!</v-toolbar-title>
      </v-toolbar>
      <main>
      <v-content>
        <v-container fluid>
          <v-row>
            <v-col sm6 key="1">
              <v-card>
                <v-card-title>Your data</v-card-title>
                <v-card-text>
                  <v-card-row height="150px">
                    <!-- ************************************************ --> 
                      
                    <textarea class="user-data" v-model="csvdata"></textarea>
                      
                    <!-- ************************************************ -->
                  </v-card-row>
                </v-card-text>
                <v-card-row actions>
                  <!-- ******************************************************* -->

                  <v-btn v-on:click.native="clear" class="grey lighten-4" default>Clear</v-btn>
                  <v-btn v-on:click.native="setDefaultData" class="grey lighten-4" default>Show example</v-btn>

                  <!-- ******************************************************* -->                  
                </v-card-row>
              </v-card>
            </v-col>

            <v-col sm6 key="2">
              <v-card>
                <v-card-title>Data table</v-card-title>
                <v-card-text>
                  <v-card-row height="150px" class="data-table">
                    <!-- ******************************************************* -->

                    <div v-if="labels.length > 0">

                      <table>
                        <tr><th v-for="label in labels">{{ label }}</th></tr>
                        <tr><td v-for="value in values">{{ value }}</td></tr>
                      </table>
                      
                    </div>

                    <div v-else class="no-data text-xs-center">No data</div>

                    <!-- ******************************************************* -->
                  </v-card-row>
                </v-card-text>
                <v-card-row actions>
                  <!-- ******************************************************* -->

                  <v-col md12>
                    <v-btn-toggle v-bind:options="chartOptions" v-model="chartType" class="right"></v-btn-toggle>
                  </v-col>

                  <!-- ******************************************************* -->                  
                </v-card-row>
              </v-card>
            </v-col>
          </v-row>
          <v-row class="mt-3">
            <v-col sm12 key="3">
              <v-card>
                <v-card-title>Nice chart !</v-card-title>
                <v-card-text>
                  <v-card-row height="203px">
                    <!-- ******************************************************* -->

                    <pie-chart v-if="chartType === 'pie'" :labels="labels" :values="values" height="250" width="250"></pie-chart>
                    <bar-chart v-if="chartType === 'bar'" :labels="labels" :values="values" height="250" width="250"></bar-chart>

                    <!-- ******************************************************* -->
                  </v-card-row>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-content>
      </main>
    </v-app>
</template>

<script type="text/javascript">

import PieChart from './PieChart.vue'
import BarChart from './BarChart.vue'

export default {
    name: 'dataview',

    data () {
        return {
            csvdata: '',
            chartType: 'pie',
            chartOptions: [
              { icon: 'pie_chart', value: 'pie', class: 'white--text' },
              { icon: 'insert_chart', value: 'bar' }
           ]
        }
    },

    components: {
        'pie-chart': PieChart, 'bar-chart': BarChart 
    },

    computed: {
        labels () {
            return this.getRow(0)
        },

        values () {
            return this.getRow(1)
        }
    },

    methods: {
        getRow (i) {
            let lines = this.csvdata.split('\n')
            if (lines.length > 1) {
                return lines[i].split(',')
            }
            return []   
        },

        clear: function () {
            this.csvdata = ''
        },

        setDefaultData: function () {
            this.csvdata = 'Chrome,Safari,UC Browser,Firefox,Others\n52.81,14.44,8.75,6.67,7.86'
        }
    }
}
</script>

<style scoped>
.content>.container {
    padding: 1rem 3rem;
}

.no-data {
    width: 100%;
}

.data-table {
  display: table;
  width: 100%;
}

.user-data {
  width: 100%;
  height: 100%;
}

th {
    max-width: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
</style>