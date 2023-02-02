<script setup>
import { computed, ref, onMounted } from "vue";
import { useMainStore } from "@/stores/main";
import {
  mdiAccountMultiple,
  mdiCartOutline,
  mdiChartTimelineVariant,
  mdiMonitorCellphone,
  mdiReload,
  mdiGithub,
  mdiChartPie,
  mdiFire,
  mdiWeightKilogram
} from "@mdi/js";
import * as chartConfig from "@/components/Charts/chart.config.js";
import LineChart from "@/components/Charts/LineChart.vue";
import SectionMain from "@/components/SectionMain.vue";
import CardBoxWidget from "@/components/CardBoxWidget.vue";
import CardBox from "@/components/CardBox.vue";
import TableSampleClients from "@/components/TableSampleClients.vue";
import NotificationBar from "@/components/NotificationBar.vue";
import BaseButton from "@/components/BaseButton.vue";
import CardBoxTransaction from "@/components/CardBoxTransaction.vue";
import CardBoxClient from "@/components/CardBoxClient.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
import SectionBannerStarOnGitHub from "@/components/SectionBannerStarOnGitHub.vue";

const chartData = ref(null);

const get_c = () => {
  chartData.value = {
    "labels": [
      "01",
      "02",
      "03",
      "04",
      "05",
      "06",
      "07",
      "08",
      "09"
    ],
    "datasets": [
      {
        "fill": false,
        "borderColor": "#00D1B2",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#00D1B2",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#00D1B2",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          25,
          52,
          124,
          39,
          83,
          108,
          180,
          21,
          65
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      },
      {
        "fill": false,
        "borderColor": "#209CEE",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#209CEE",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#209CEE",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          123,
          99,
          190,
          98,
          93,
          27,
          136,
          6,
          158
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      },
      {
        "fill": false,
        "borderColor": "#FF3860",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#FF3860",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#FF3860",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          2,
          9,
          61,
          13,
          137,
          117,
          37,
          3,
          179
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      }
    ]
  };
}
const fillChartData = () => {
  chartData.value = {
    "labels": [
      "01",
      "02",
      "03",
      "04",
      "05",
      "06",
      "07",
      "08",
      "09"
    ],
    labels: ['January', 'February', 'March', 'April', 'May'],
    "datasets": [
      {
        "fill": false,
        "label": 'test',
        "borderColor": "#00D1B2",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#00D1B2",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#00D1B2",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          25,
          52,
          124,
          39,
          83,
          108,
          180,
          21,
          65
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      },
      {
        "fill": false,
        "label": "two",
        "borderColor": "#209CEE",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#209CEE",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#209CEE",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          123,
          99,
          190,
          98,
          93,
          27,
          136,
          6,
          158
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      },
      {
        "fill": false,
        "label": "three",
        "borderColor": "#FF3860",
        "borderWidth": 2,
        "borderDash": [],
        "borderDashOffset": 0,
        "pointBackgroundColor": "#FF3860",
        "pointBorderColor": "rgba(255,255,255,0)",
        "pointHoverBackgroundColor": "#FF3860",
        "pointBorderWidth": 20,
        "pointHoverRadius": 4,
        "pointHoverBorderWidth": 15,
        "pointRadius": 4,
        "data": [
          2,
          9,
          61,
          13,
          137,
          117,
          37,
          3,
          1
        ],
        "tension": 0.5,
        "cubicInterpolationMode": "default"
      }
    ],

    options: {
        legend: {
            display: true,
            labels: {
                fontColor: 'rgb(255, 99, 132)'
            }
        }
    }


  };

};




onMounted(() => {
  fillChartData();
});



const mainStore = useMainStore();

const clientBarItems = computed(() => mainStore.clients.slice(0, 4));

const transactionBarItems = computed(() => mainStore.history);

</script>

<template>
  <LayoutAuthenticated>
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiChartTimelineVariant" title="Overview" main>
        <BaseButton href="https://github.com/justboil/admin-one-vue-tailwind" target="_blank" :icon="mdiGithub"
          label="Star on GitHub" color="contrast" rounded-full small />
      </SectionTitleLineWithButton>

      <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 mb-6">
        <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire" :number="today_cal"
          suffix="kcal" label="하루 권장 열량" />
        <CardBoxWidget trend="12%" trend-type="down" color="text-black-500" :icon="mdiWeightKilogram"
          :number="today_weight" suffix="kg" label="몸무게" />
        <CardBoxWidget trend="Overflow" trend-type="alert" color="text-red-500" :icon="mdiFire" :number="today_remain"
          suffix="kcal" label="남은 섭취 가능 열량" />
      </div>
      <!-- 
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
        <div class="flex flex-col justify-between">
          <CardBoxTransaction
            v-for="(transaction, index) in transactionBarItems"
            :key="index"
            :amount="transaction.amount"
            :date="transaction.date"
            :business="transaction.business"
            :type="transaction.type"
            :name="transaction.name"
            :account="transaction.account"
          />
        </div>
        <div class="flex flex-col justify-between">
          <CardBoxClient
            v-for="client in clientBarItems"
            :key="client.id"
            :name="client.name"
            :login="client.login"
            :date="client.created"
            :progress="client.progress"
          />
        </div>
      </div> -->

      <!-- <SectionBannerStarOnGitHub class="mt-6 mb-6" /> -->

      <SectionTitleLineWithButton :icon="mdiChartPie" title="Trends overview">
        <BaseButton :icon="mdiReload" color="whiteDark" v-on:click="get_c()" />
      </SectionTitleLineWithButton>

      <CardBox class="mb-6">
        <div v-if="chartData">
          <line-chart :data="chartData" class="h-96" />
        </div>
      </CardBox>

      <!-- <SectionTitleLineWithButton :icon="mdiAccountMultiple" title="Clients" /> -->

      <!-- <NotificationBar color="info" :icon="mdiMonitorCellphone">
        <b>Responsive table.</b> Collapses on mobile
      </NotificationBar> -->

      <!-- <CardBox has-table>
        <TableSampleClients />
      </CardBox> -->
    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>
import axios from 'axios';
import {
  Chart,
  LineElement,
  PointElement,
  LineController,
  LinearScale,
  CategoryScale,
  Tooltip,
} from "chart.js";

export default {
  name: 'LoginView',
  components: {
  },
  data() {
    return {
      today_cal: 3000,
      today_weight: 70,
      today_remain: 1000
    }
  },
  methods: {
    test: function () {
      console.log(1);
    },
    getchart: function () {

      var myChart = {
        "labels": [
          "01",
          "02",
          "03",
          "04",
          "05",
          "06",
          "07",
          "08",
          "09"
        ],
        "datasets": [
          {
            "fill": false,
            "borderColor": "#00D1B2",
            "borderWidth": 2,
            "borderDash": [],
            "borderDashOffset": 0,
            "pointBackgroundColor": "#00D1B2",
            "pointBorderColor": "rgba(255,255,255,0)",
            "pointHoverBackgroundColor": "#00D1B2",
            "pointBorderWidth": 20,
            "pointHoverRadius": 4,
            "pointHoverBorderWidth": 15,
            "pointRadius": 4,
            "data": [
              114,
              176,
              170,
              146,
              172,
              129,
              78,
              42,
              160
            ],
            "tension": 0.5,
            "cubicInterpolationMode": "default"
          },
          {
            "fill": false,
            "borderColor": "#209CEE",
            "borderWidth": 2,
            "borderDash": [],
            "borderDashOffset": 0,
            "pointBackgroundColor": "#209CEE",
            "pointBorderColor": "rgba(255,255,255,0)",
            "pointHoverBackgroundColor": "#209CEE",
            "pointBorderWidth": 20,
            "pointHoverRadius": 4,
            "pointHoverBorderWidth": 15,
            "pointRadius": 4,
            "data": [
              171,
              121,
              51,
              107,
              167,
              143,
              62,
              188,
              142
            ],
            "tension": 0.5,
            "cubicInterpolationMode": "default"
          },
          {
            "fill": false,
            "borderColor": "#FF3860",
            "borderWidth": 2,
            "borderDash": [],
            "borderDashOffset": 0,
            "pointBackgroundColor": "#FF3860",
            "pointBorderColor": "rgba(255,255,255,0)",
            "pointHoverBackgroundColor": "#FF3860",
            "pointBorderWidth": 20,
            "pointHoverRadius": 4,
            "pointHoverBorderWidth": 15,
            "pointRadius": 4,
            "data": [
              90,
              80,
              171,
              130,
              162,
              153,
              65,
              58,
              177
            ],
            "tension": 0.5,
            "cubicInterpolationMode": "default"
          }
        ]
      }
      this.chartData.value = myChart;
      console.log(this.chartData);
    },
  }

}
</script>
