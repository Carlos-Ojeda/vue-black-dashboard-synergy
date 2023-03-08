<template>
  <div>

    <div class="row">
      <div class="col-12">
        <card type="chart">
          <template slot="header">
            <div class="row">
              <div class="col-sm-6" :class="isRTL ? 'text-right' : 'text-left'">
                <h5 class="card-category">Personas promedio</h5>
                <h2 class="card-title">Personas</h2>
              </div>
              <div class="col-sm-6">
                <div class="btn-group btn-group-toggle"
                    :class="isRTL ? 'float-left' : 'float-right'"
                    data-toggle="buttons">
                  <label v-for="(option, index) in bigLineChartCategories"
                        :key="option"
                        class="btn btn-sm btn-primary btn-simple"
                        :class="{active: bigLineChart.activeIndex === index}"
                        :id="index">
                    <input type="radio"
                          @click="initBigChart(index)"
                          name="options" autocomplete="off"
                          :checked="bigLineChart.activeIndex === index">
                    {{option}}
                  </label>
                </div>
              </div>
            </div>
          </template>
          <div class="chart-area">
            <line-chart style="height: 220px"
                        ref="bigChart"
                        chart-id="big-line-chart"
                        :chart-data="bigLineChart.chartData"
                        :gradient-colors="bigLineChart.gradientColors"
                        :extra-options="bigLineChart.extraOptions"
                        >
            </line-chart>
          </div>
        </card>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-4" :class="{'text-right': isRTL}">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">Scans Totales</h5>
            <h3 class="card-title"><i class="tim-icons icon-camera-18 text-primary "></i> 259</h3>
          </template>
          <div class="chart-area">
            <line-chart style="height: 100%"
                        chart-id="purple-line-chart"
                        :chart-data="purpleLineChart.chartData"
                        :gradient-colors="purpleLineChart.gradientColors"
                        :gradient-stops="purpleLineChart.gradientStops"
                        :extra-options="purpleLineChart.extraOptions">
            </line-chart>
          </div>
        </card>
      </div>
      <div class="col-lg-4" :class="{'text-right': isRTL}">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">Compras del libro</h5>
            <h3 class="card-title"><i class="tim-icons icon-cart text-info "></i> ¿Se interactuo con los QR?</h3>
          </template>
          <barch :data-synergy="dataSynergyCompra"></barch>
        </card>
      </div>
      <div class="col-lg-4" :class="{'text-right': isRTL}">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">{{$t('dashboard.completedTasks')}}</h5>
            <h3 class="card-title"><i class="tim-icons icon-send text-success "></i> 12,100K</h3>
          </template>
          <div class="chart-area">
            <line-chart style="height: 100%"
                        chart-id="green-line-chart"
                        :chart-data="greenLineChart.chartData"
                        :gradient-stops="greenLineChart.gradientStops"
                        :extra-options="greenLineChart.extraOptions">
            </line-chart>
          </div>
        </card>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-6 col-md-12">
        <card type="tasks" :header-classes="{'text-right': isRTL}">
          <template slot="header">
            <h6 class="title d-inline">{{$t('dashboard.tasks', {count: 5})}}</h6>
            <p class="card-category d-inline">{{$t('dashboard.today')}}</p>
            <base-dropdown menu-on-right=""
                          tag="div"
                          title-classes="btn btn-link btn-icon"
                          aria-label="Settings menu"
                          :class="{'float-left': isRTL}">
              <i slot="title" class="tim-icons icon-settings-gear-63"></i>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.action')}}</a>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.anotherAction')}}</a>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.somethingElse')}}</a>
            </base-dropdown>
          </template>
          <div class="table-full-width table-responsive">
            <task-list></task-list>
          </div>
        </card>
      </div>
      <div class="col-lg-6 col-md-12">
        <card class="card" :header-classes="{'text-right': isRTL}">
          <h4 slot="header" class="card-title">QR escaneados</h4>
          <div class="table-responsive">
            <user-table></user-table>
          </div>
        </card>
      </div>
    </div>
  </div>
</template>
<script>
  import LineChart from '@/components/Charts/LineChart';
  import BarChart from '@/components/Charts/BarChart';
  import * as chartConfigs from '@/components/Charts/config';
  import TaskList from './Dashboard/TaskList';
  import UserTable from './Dashboard/UserTable';
  import config from '@/config';
  import barch from './Dashboard/barch.vue';
  export default {
    components: {
      LineChart,
      BarChart,
      TaskList,
      UserTable,
      barch
    },
    data() {
      return {
        dataSynergyCompra:[ 
            { "os": 1, "city": 6, "country": "Mexico", "timestamp": 1678075066, "ip": "200.36.251.35", "qr": 2 }, 
            { "qr": 1, "city": 15, "os": 1, "ip": "200.36.251.158", "timestamp": 1678024092, "country": "Mexico" }, 
            { "ip": "200.36.251.247", "qr": 0, "timestamp": 1677911336, "country": "Mexico", "city": 17, "os": 1 }, 
            { "qr": 1, "os": 0, "city": 5, "ip": "200.36.251.56", "country": "Mexico", "timestamp": 1678234956 }, 
            { "timestamp": 1678229859, "os": 0, "qr": 0, "ip": "200.36.251.128", "city": 18, "country": "Mexico" } 
          ],
        bigLineChart: {
          extraOptions: chartConfigs.purpleChartOptions2,
          chartData: {
            
            datasets: [{
              label: "Ventas",
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [{x:1678229856000,y:1}, {x:1678229859000, y:2}, {x:1678229879000,y:3}, {x:1678229889000,y:4}, {x:1678229899000,y:5}, {x:1678229909000,y:1}],
            },
            {
              label: "personas",
              fill: true,
              borderColor: config.colors.info,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.info,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.info,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [{x:1678229856100,y:2}, {x:1678229859200, y:4}, {x:1678229879300,y:6}, {x:1678229889100,y:1}, {x:1678229899000,y:2}, {x:1678229909500,y:7}],
            }
          ]
          },
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.2, 0],
        },
        purpleLineChart: {
          extraOptions: chartConfigs.purpleChartOptions2,
          chartData: {
            
            datasets: [{
              label: "Qr escaneados",
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [{x:1678229856000,y:1}, {x:1678229859000, y:2}, {x:1678229879000,y:3}, {x:1678229889000,y:4}, {x:1678229899000,y:5}, {x:1678229909000,y:1}],
            },
          ]
          },
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.2, 0],
        },
        greenLineChart: {
          extraOptions: chartConfigs.greenChartOptions,
          chartData: {
            labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV'],
            datasets: [{
              label: "My First dataset",
              fill: true,
              borderColor: config.colors.danger,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.danger,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.danger,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [90, 27, 60, 12, 80],
            }]
          },
          gradientColors: ['rgba(66,134,121,0.15)', 'rgba(66,134,121,0.0)', 'rgba(66,134,121,0)'],
          gradientStops: [1, 0.4, 0],
        },
        blueBarChart: {
          extraOptions: chartConfigs.barChartOptions,
          chartData: {
            labels: ['USA', 'GER', 'AUS', 'UK', 'RO', 'BR'],
            datasets: [{
              label: "Si",
              fill: true,
              borderColor: config.colors.info,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              data: [1, 2, 3, 4, 5, 6],
            },
            {
              label: "No",
              fill: true,
              borderColor: "#42b882",
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              data: [6, 5, 4, 3, 2, 1],
            },
          ]
          },
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.4, 0],
        }
      }
    },
    computed: {
      enableRTL() {
        return this.$route.query.enableRTL;
      },
      isRTL() {
        return this.$rtl.isRTL;
      },
      bigLineChartCategories() {
        return this.$t('dashboard.chartCategories');
      }
    },
    methods: {
      initBigChart(index) {
        let chartData = {
          
          datasets: [{
            
            fill: true,
            borderColor: config.colors.primary,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: config.colors.primary,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: config.colors.primary,
            data: this.bigLineChart.allData[index]
          },
          
        ],
        }
        this.$refs.bigChart.updateGradients(chartData);
        this.bigLineChart.chartData = chartData;
        this.bigLineChart.activeIndex = index;
      }
    },
    mounted() {
      this.i18n = this.$i18n;
      if (this.enableRTL) {
        this.i18n.locale = 'ar';
        this.$rtl.enableRTL();
      }
      this.initBigChart(0);
    },
    beforeDestroy() {
      if (this.$rtl.isRTL) {
        this.i18n.locale = 'en';
        this.$rtl.disableRTL();
      }
    }
  };
</script>
<style>
</style>
