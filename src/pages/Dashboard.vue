<template>
  <div>

    <div class="row">
      <div class="col-12">
        <card type="chart">
          <template slot="header">
            <div class="row">
              <div class="col-sm-6" :class="isRTL ? 'text-right' : 'text-left'">
                <!-- <h5 class="card-category">{{$t('dashboard.totalShipments')}}</h5> -->
                <h2 class="card-title">{{$t('dashboard.performance')}}</h2>
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
            <line-chart style="height: 100%"
                        ref="bigChart"
                        chart-id="big-line-chart"
                        :chart-data="bigLineChart.chartData"
                        :gradient-colors="bigLineChart.gradientColors"
                        :gradient-stops="bigLineChart.gradientStops"
                        :extra-options="bigLineChart.extraOptions">
            </line-chart>
          </div>
        </card>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-4" :class="{'text-right': isRTL}">
        <card type="task" v-bind:style="{'height': '470px'}">
          <template slot="header">
            <h3 class="card-title">{{$t('dashboard.apiProxyDetails.heading')}}</h3>
            <!-- <h3 class="card-title"><i class="tim-icons icon-bell-55 text-primary "></i> 763,215</h3> -->
           <div class="chart-area">
            <nav>
             <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <a class="nav-item nav-link active title d-inline" id="nav-api-tab" data-toggle="tab" href="#nav-api"  role="tab" aria-controls="nav-api" aria-selected="true">API</a>
            <a  class="nav-item nav-link title d-inline"  id="nav-api-operation-tab" data-toggle="tab" href="#nav-api-operation" role="tab" aria-controls="nav-api-operation" aria-selected="true">operation</a>
             <a  class="nav-item nav-link title d-inline"  id="nav-api-user-tab" data-toggle="tab" href="#nav-api-user" role="tab" aria-controls="nav-api-user" aria-selected="true">user</a>
              <a  class="nav-item nav-link title d-inline"  id="nav-api-product-tab" data-toggle="tab" href="#nav-api-product" role="tab" aria-controls="nav-api-product" aria-selected="true">product</a>
          </div>
          </nav>
          </div>
          </template>

           <div class="tab-content" id="nav-tabContent">
              <div class="tab-pane fade show active" id="nav-api" role="tabpanel" aria-labelledby="nav-api-tab">
               <p class="text">API Proxy Version: {{$t('dashboard.apiProxyDetails.api.version')}}</p>
                <!-- <p class="text">API Proxy to: {{$t('dashboard.apiProxyDetails.api.to')}}</p> -->
                 <p class="text">API Proxy from: {{$t('dashboard.apiProxyDetails.api.from')}}</p>
                  <p class="text">API Proxy revision: {{$t('dashboard.apiProxyDetails.api.revision')}}</p>
              </div>
              <div class="tab-pane fade" id="nav-api-operation" role="tabpanel" aria-labelledby="nav-api-operation-tab">
                <!-- <p class="text-muted">Message: {{row.description}}</p> -->

                <p class="text">API Proxy method: {{$t('dashboard.apiProxyDetails.operation.method')}}</p>
                <!-- <p class="text">API Proxy to: {{$t('dashboard.apiProxyDetails.api.to')}}</p> -->
                 <p class="text">API Proxy URI : {{$t('dashboard.apiProxyDetails.operation.uriTemplate')}}</p>
                 
              </div>
              <div class="tab-pane fade" id="nav-api-user" role="tabpanel" aria-labelledby="nav-api-user-tab">
                <!-- <p class="text-muted">Message: {{row.description}}</p> -->
                <p class="text" v-if="$t('dashboard.apiProxyDetails.user') === '-'">There is no user</p>
                <p class="text" v-else>API Proxy user: {{$t('dashboard.apiProxyDetails.user')}}</p>
                
                 
              </div>

              <div class="tab-pane fade" id="nav-api-product" role="tabpanel" aria-labelledby="nav-api-product-tab">
                <!-- <p class="text-muted">Message: {{row.description}}</p> -->
                <p class="text-muted" v-if="$t('dashboard.apiProxyDetails.product') === '-'">There is no product assigned</p>
                <p class="text-muted" v-else>API Proxy product: {{$t('dashboard.apiProxyDetails.product')}}</p>
                
                 
              </div>
            </div>
          <!-- <div class="chart-area">
            <line-chart style="height: 100%"
                        chart-id="purple-line-chart"
                        :chart-data="purpleLineChart.chartData"
                        :gradient-colors="purpleLineChart.gradientColors"
                        :gradient-stops="purpleLineChart.gradientStops"
                        :extra-options="purpleLineChart.extraOptions">
            </line-chart>
          </div> -->
        </card>
      </div>
       <div class="col-lg-8" :class="{'text-right': isRTL}">
        <card type="tasks">
          <template slot="header">
             <h3 class="card-title">API Proxy Headers</h3>
            <!-- <h3 class="card-title"><i class="tim-icons icon-bell-55 text-primary "></i> 763,215</h3> -->
           
            <nav>
             <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <a class="nav-item nav-link active title d-inline" id="nav-request-headers-tab" data-toggle="tab" href="#nav-request-headers"  role="tab" aria-controls="nav-request-headers" aria-selected="true">Inbound Headers</a>
            <a  class="nav-item nav-link title d-inline"  id="nav-backend-request-headers-tab" data-toggle="tab" href="#nav-backend-request-headers" role="tab" aria-controls="nav-backend-request-headers" aria-selected="true">Backend Headers</a>
             <a  class="nav-item nav-link title d-inline"  id="nav-outbound-request-headers-tab" data-toggle="tab" href="#nav-outbound-request-headers" role="tab" aria-controls="nav-outbound-request-headers" aria-selected="true">Outbound Headers</a>
          </div>
          </nav>
         
          </template>
             <div class="table-full-width " v-bind:style="{'overflow-y':'scroll', 'height': '360px'}">
           <div class="tab-content" id="nav-tabContent">
              <div class="tab-pane fade show active" id="nav-request-headers" role="tabpanel" aria-labelledby="nav-request-headers-tab">
               <!-- <p class="text">API Proxy Version: {{$t('dashboard.apiProxyDetails.api.version')}}</p> -->
                <!-- <p class="text">API Proxy to: {{$t('dashboard.apiProxyDetails.api.to')}}</p> -->
                  <request-headers></request-headers>
              </div>
              <div class="tab-pane fade" id="nav-backend-request-headers" role="tabpanel" aria-labelledby="nav-backend-request-headers-tab">
                <!-- <p class="text-muted">Message: {{row.description}}</p> -->

                <backend-request-headers></backend-request-headers>
                 
              </div>
            
            <div class="tab-pane fade" id="nav-outbound-request-headers" role="tabpanel" aria-labelledby="nav-outbound-request-headers-tab">
                <!-- <p class="text-muted">Message: {{row.description}}</p> -->

                 <backend-request-headers></backend-request-headers>
                 
              </div>
            </div>
            </div>
          <!-- <div class="chart-area">
            <line-chart style="height: 100%"
                        chart-id="purple-line-chart"
                        :chart-data="purpleLineChart.chartData"
                        :gradient-colors="purpleLineChart.gradientColors"
                        :gradient-stops="purpleLineChart.gradientStops"
                        :extra-options="purpleLineChart.extraOptions">
            </line-chart>
          </div> -->
        </card>
      </div>
     
      <!-- <div class="col-lg-4" :class="{'text-right': isRTL}">
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
      </div> -->
    </div>
    <div class="row">
      <div class="col-lg-6 col-md-12">
        <card type="tasks" :header-classes="{'text-right': isRTL}">
          <template slot="header">
            <h3 class="card-title">API Proxy Steps</h3>
            <!--  <nav>
  <div class="nav nav-tabs" id="nav-tab" role="tablist">
    <a class="nav-item nav-link active" id="nav-home-tab" data-toggle="tab" href="#nav-home" role="tab" aria-controls="nav-home" aria-selected="true">Home</a>
    <a class="nav-item nav-link" id="nav-profile-tab" data-toggle="tab" href="#nav-profile" role="tab" aria-controls="nav-profile" aria-selected="false">Profile</a>
    <a class="nav-item nav-link" id="nav-contact-tab" data-toggle="tab" href="#nav-contact" role="tab" aria-controls="nav-contact" aria-selected="false">Contact</a>
  </div>
</nav>
<div class="tab-content" id="nav-tabContent">
  <div class="tab-pane fade show active" id="nav-home" role="tabpanel" aria-labelledby="nav-home-tab">helllo gullu</div>
  <div class="tab-pane fade" id="nav-profile" role="tabpanel" aria-labelledby="nav-profile-tab">...</div>
  <div class="tab-pane fade" id="nav-contact" role="tabpanel" aria-labelledby="nav-contact-tab">see yaa</div>
</div> -->
            <nav>
             <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <a class="nav-item nav-link active title d-inline" id="nav-inbound-tab" data-toggle="tab" href="#nav-inbound"  role="tab" aria-controls="nav-inbound" aria-selected="true">{{$t('dashboard.inbound', {count: 5})}}</a>
            <a  class="nav-item nav-link title d-inline"  id="nav-outbound-tab" data-toggle="tab" href="#nav-out" role="tab" aria-controls="nav-outbound" aria-selected="true">{{$t('dashboard.outbound', {count: 5})}}</a>
          </div>
          </nav>
           <!--  <p class="card-category d-inline">{{$t('dashboard.today')}}</p> -->
            <!-- <base-dropdown menu-on-right=""
                           tag="div"
                           title-classes="btn btn-link btn-icon"
                           aria-label="Settings menu"
                           :class="{'float-left': isRTL}">
              <i slot="title" class="tim-icons icon-settings-gear-63"></i>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.action')}}</a>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.anotherAction')}}</a>
              <a class="dropdown-item" href="#pablo">{{$t('dashboard.dropdown.somethingElse')}}</a>
            </base-dropdown> -->
          </template>
         <!--  <div class="tab-content" id="nav-tabContent">
              <div class="tab-pane fade show active" id="nav-inbound" role="tabpanel" aria-labelledby="nav-inbound-tab">
              <task-list ></task-list>
              </div>
              <div class="tab-pane fade" id="nav-out" role="tabpanel" aria-labelledby="nav-outbound-tab">
               <user-table></user-table>
              </div>
            </div> -->


          <div class="table-full-width " v-bind:style="{'overflow-y':'scroll', 'height': '350px'}">
            <div class="tab-content" id="nav-tabContent">
              <div class="tab-pane fade show active" id="nav-inbound" role="tabpanel" aria-labelledby="nav-inbound-tab">
              <task-list ></task-list>
              </div>
              <div class="tab-pane fade" id="nav-out" role="tabpanel" aria-labelledby="nav-outbound-tab">
              <out-bound ></out-bound>
              </div>
            </div>
          </div>
        </card>
      </div>
      <div class="col-lg-6 col-md-12">
        <card  type="tasks" :header-classes="{'text-right': isRTL}">
          <template slot="header">
          <nav>
             <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <a class="nav-item nav-link active title d-inline" id="nav-policy-reference-tab" data-toggle="tab" href="#nav-policy-reference"  role="tab" aria-controls="nav-policy-reference" aria-selected="true">policy-reference</a>
            <a  class="nav-item nav-link title d-inline"  id="nav-example-tab" data-toggle="tab" href="#nav-outbound" role="tab" aria-controls="nav-outbound" aria-selected="true">example</a>
          </div>
          </nav>
        </template>
         
          <div class="table-full-width " v-bind:style="{'overflow-y':'scroll', 'height': '402px'}" >
             <div class="tab-pane fade show active" id="nav-policy-reference" role="tabpanel" aria-labelledby="nav-policy-reference-tab">
               <user-table></user-table>
              </div>
           
          </div>
        </card>
      </div>
    </div>
  </div>
</template>

<script >

  import LineChart from '@/components/Charts/LineChart';
  import BarChart from '@/components/Charts/BarChart';
  import * as chartConfigs from '@/components/Charts/config';
  import TaskList from './Dashboard/TaskList';
  import UserTable from './Dashboard/UserTable';
  import OutBound from './Dashboard/OutBound';
  import RequestHeaders from './Dashboard/RequestHeaders';
  import BackendRequestHeaders from  './Dashboard/RequestHeaders'
  import OutBoundRequestHeaders from  './Dashboard/RequestHeaders'
  import config from '@/config';

  export default {
    components: {
      LineChart,
      BarChart,
      TaskList,
      UserTable,
      OutBound,
      RequestHeaders,
      BackendRequestHeaders,
      OutBoundRequestHeaders
    },
    data() {
      return {
        bigLineChart: {
          allData: [
            [100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100.100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100],
            [80, 120, 105, 110, 95, 105, 90, 100, 80, 95, 70, 120,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100],
            [100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100.100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100,100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100],
          ],
          activeIndex: 0,
          chartData: {
            datasets: [{ }]
          },
          extraOptions: chartConfigs.purpleChartOptions,
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.4, 0],
          categories: []
        },
        // purpleLineChart: {
        //   extraOptions: chartConfigs.purpleChartOptions,
        //   chartData: {
        //     labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
        //     datasets: [{
        //       label: "Data",
        //       fill: true,
        //       borderColor: config.colors.primary,
        //       borderWidth: 2,
        //       borderDash: [],
        //       borderDashOffset: 0.0,
        //       pointBackgroundColor: config.colors.primary,
        //       pointBorderColor: 'rgba(255,255,255,0)',
        //       pointHoverBackgroundColor: config.colors.primary,
        //       pointBorderWidth: 20,
        //       pointHoverRadius: 4,
        //       pointHoverBorderWidth: 15,
        //       pointRadius: 4,
        //       data: [80, 100, 70, 80, 120, 80],
        //     }]
        //   },
        //   gradientColors: config.colors.primaryGradient,
        //   gradientStops: [1, 0.2, 0],
        // },
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
        }
        // blueBarChart: {
        //   extraOptions: chartConfigs.barChartOptions,
        //   chartData: {
        //     labels: ['USA', 'GER', 'AUS', 'UK', 'RO', 'BR'],
        //     datasets: [{
        //       label: "Countries",
        //       fill: true,
        //       borderColor: config.colors.info,
        //       borderWidth: 2,
        //       borderDash: [],
        //       borderDashOffset: 0.0,
        //       data: [53, 20, 10, 80, 100, 45],
        //     }]
        //   },
        //   gradientColors: config.colors.primaryGradient,
        //   gradientStops: [1, 0.4, 0],
        // }
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
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.bigLineChart.allData[index]
          }],
          labels: ['06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45','06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45','06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45','06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45','06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45','06:00', '07:00','08:00', '09:00', '10:00', '11:00','11:45','12:00', '12:10','12:34', '12:32', '12:12', '12:00','11:45'],
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
