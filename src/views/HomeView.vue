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
  mdiWeightKilogram,
  mdiRun,
  mdiCalendarToday,
  mdiRice,
  mdiFoodDrumstick,
  mdiPigVariantOutline
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
import { get_days, get_months } from "@/rest/chartconfig.js";

import SectionFullScreen from "@/components/SectionFullScreen.vue";

import LoadingSpinner from "@/components/LoadingSpinner.vue";

// to do 
let now = new Date("2022-09-01");

const chartData = ref(null);

const chartData2 = ref(null);

const loading_c = ref(null);

loading_c.value = false;

const fillChartData_days = async () => {

  var rrr = await get_days()

  chartData.value = rrr;
};

const fillChartData_months = async () => {

  var rrr = await get_months()

  chartData2.value = rrr;
};

const today_sum = ref(null);
const activity_sum = ref(null);
const nutrient_sum = ref(null);
const weight_trend = ref(null);
const nutrient_trend = ref(null);
const yes_nutrient_sum = ref(null);
const activity_warn = ref(null);


weight_trend.value = ['13%', 'down']
today_sum.value = { "today_cal": 3000, "today_weight": 70, "today_remain": 1000 };
nutrient_sum.value = { 'fat': 300, 'protein': 400, 'carbo': 300 };
yes_nutrient_sum.value = { 'fat': 300, 'protein': 400, 'carbo': 300 };
nutrient_trend.value = { 'fat': ['12%', 'down'], 'protein': ['13%', 'up'], 'carbo': ['0%', 'mid'] };

activity_sum.value = { 'today_activity': 1000, 'today_activity_goal': 500 };

activity_warn.value = ['Good!', 'success'];

const get_today = async () => {
  try {
    var response = await axios.get('http://localhost:3001/api/profile', {
      withCredentials: true,
    });
    console.log(response)
    today_sum.value['today_cal'] = response.data[0].kcal_per_day;
    today_sum.value['today_weight'] = response.data[0].current_weight;
    let now = new Date("2022-09-01");



  } catch (err) {
    console.log("Error >>", err);
  }

  try {
    var response = await axios.get("http://localhost:3001/api/today/total", {
      params: { date: now },
      withCredentials: true,
    });
    var n_in = { 'fat': 0, 'protein': 0, 'carbo': 0 }
    var total_eat = 0;
    console.log(response.data);
    for (var i = 0; i < response.data[0].length; i++) {
      n_in['fat'] += response.data[0][i]['fat'];
      n_in['protein'] += response.data[0][i]['protein'];
      n_in['carbo'] += response.data[0][i]['carbo'];
      total_eat += response.data[0][i]['kcal'];
    }
    nutrient_sum.value = n_in;
    var n_in = { 'fat': 0, 'protein': 0, 'carbo': 0 }
    var yes_total_eat = 0;
    console.log("?????? >> ", response);
    for (var i = 0; i < response.data[1].length; i++) {
      n_in['fat'] += response.data[1][i]['fat'] * response.data[1][i]['gram'];
      n_in['protein'] += response.data[1][i]['protein'] * response.data[1][i]['gram'];
      n_in['carbo'] += response.data[1][i]['carbo'] * response.data[1][i]['gram'];
      yes_total_eat += response.data[0][i]['kcal'] * response.data[0][i]['gram'];
    }
    yes_nutrient_sum.value = n_in;
    console.log('yes', yes_nutrient_sum.value);

    var label_list = ['fat', 'protein', 'carbo']
    for (var i = 0; i < label_list.length; i++) {
      var percent = Math.round((nutrient_sum.value[label_list[i]] - yes_nutrient_sum.value[label_list[i]]) * 100 / (yes_nutrient_sum.value[label_list[i]]));
      nutrient_trend.value[label_list[i]][0] = percent + '%'
      if (percent > 0) {
        nutrient_trend.value[label_list[i]][1] = 'up';
      } else if (percent < 0) {
        nutrient_trend.value[label_list[i]][1] = 'down';
      } else {
        nutrient_trend.value[label_list[i]][1] = 'mid';
      }

    }

    var yes_weight = response.data[2][0].weight;
    var w_percent = Math.round((today_sum.value.today_weight - yes_weight) * 100 / yes_weight)
    weight_trend.value[0] = w_percent + '%'

    if (w_percent > 0) {
      weight_trend.value[1] = 'up'
    } else if (w_percent < 0) {
      weight_trend.value[1] = 'down'
    } else {
      weight_trend.value[1] = 'mid'
    }

    var activity_in = { 'today_activity': 0, 'today_activity_goal': 0 };

    for (var i = 0; i < response.data[3].length; i++) {
      activity_in['today_activity'] += Math.round(response.data[3][i]['working_time'] * response.data[3][i]['coefficient'] * today_sum.value['today_weight'] / 15);
    }

    activity_sum.value = activity_in;


    // to do ???????????? ?????? ?????? ?????? ????????? ?????? ??????(??????)
    today_sum.value['today_remain'] = today_sum.value['today_cal'] - total_eat + activity_in['today_activity'];
    console.log("today_remain", today_sum.value['today_remain']);


    if (today_sum.value['today_remain'] >= 0) {
      activity_sum.value['today_activity_goal'] = 0;
    } else {

      activity_sum.value['today_activity_goal'] -= today_sum.value['total_remain'];

      activity_warn.value[0] = "Warning!";
      activity_warn.value[1] = "alert";
    }

    console.log("today total >>", response.data[3]);
    loading_c.value = true;
  } catch (err) {
    console.log("Error >>", err);
  }

  loading_c.value = true;
};

get_today();



const get_sum = function () {
  console.log(today_sum['today_cal'])
}

onMounted(() => {
  fillChartData_days();
  fillChartData_months();

});



const mainStore = useMainStore();

const clientBarItems = computed(() => mainStore.clients.slice(0, 4));

const transactionBarItems = computed(() => mainStore.history);

</script>

<template>
  <LayoutAuthenticated @change="change">
    <div v-if="!loading_c">
      <SectionFullScreen v-slot="{ cardClass }" bg="greenBlue">
        <CardBox :class="cardClass">
          <div class="space-y-3">
            <h1 class="text-2xl" style="text-align: center"></h1>
            <LoadingSpinner style="text-align: center" />

            <p style="text-align: center">Loading...</p>
          </div>
        </CardBox>
      </SectionFullScreen>
    </div>
    <div v-if="loading_c">
      <SectionMain>
        <!-- <SectionTitleLineWithButton :icon="mdiChartTimelineVariant" title="Overview" main>
        <BaseButton href="https://github.com/justboil/admin-one-vue-tailwind" target="_blank" :icon="mdiGithub"
          label="Star on GitHub" color="contrast" rounded-full small />
      </SectionTitleLineWithButton> -->
        <SectionTitleLineWithButton :icon="mdiCalendarToday" title="?????? ??????">
          <BaseButton :icon="mdiReload" color="whiteDark" @click="get_today" />
        </SectionTitleLineWithButton>

        <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 mb-6">
          <CardBoxWidget trend="info" trend-type="success" color="text-red-500" :icon="mdiFire"
            :number="today_sum['today_cal']" suffix="kcal" label="?????? ?????? ?????? ??????" />
          <CardBoxWidget :trend="weight_trend[0]" :trend-type="weight_trend[1]" color="text-black-500"
            :icon="mdiWeightKilogram" :number="today_sum['today_weight']" suffix="kg" label="?????????" />
          <CardBoxWidget trend="Info" trend-type="success" color="text-red-500" :icon="mdiFire"
            :number="today_sum['today_remain']" suffix="kcal" label="?????? ?????? ?????? ??????" />
        </div>

        <SectionTitleLineWithButton :icon="mdiCalendarToday" title="?????? ????????? ??????">
          <BaseButton :icon="mdiReload" color="whiteDark" />
        </SectionTitleLineWithButton>

        <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 mb-6">
          <CardBoxWidget :trend="nutrient_trend['carbo'][0]" :trend-type="nutrient_trend['carbo'][1]"
            color="text-lime-400" :icon="mdiRice" :number="nutrient_sum['carbo']" suffix="g" label="????????????" />
          <CardBoxWidget :trend="nutrient_trend['protein'][0]" :trend-type="nutrient_trend['protein'][1]"
            color="text-orange-800" :icon="mdiFoodDrumstick" :number="nutrient_sum['protein']" suffix="g" label="?????????" />
          <CardBoxWidget :trend="nutrient_trend['fat'][0]" :trend-type="nutrient_trend['fat'][1]" color="text-rose-300"
            :icon="mdiPigVariantOutline" :number="nutrient_sum['fat']" suffix="g" label="??????" />
        </div>

        <SectionTitleLineWithButton :icon="mdiRun" title="?????? ??????">
          <BaseButton :icon="mdiReload" color="whiteDark" />
        </SectionTitleLineWithButton>

        <div class="grid grid-cols-1 gap-6 lg:grid-cols-2 mb-6">
          <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire"
            :number="activity_sum['today_activity']" suffix="kcal" label="?????? ?????? ??????" />
          <CardBoxWidget :trend="activity_warn[0]" :trend-type="activity_warn[1]" color="text-red-500" :icon="mdiFire"
            :number="activity_sum['today_activity_goal']" suffix="kcal" label="?????? ?????????" />
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

        <SectionTitleLineWithButton :icon="mdiChartPie" title="Trends overview - ?????? 9???">
          <BaseButton :icon="mdiReload" color="whiteDark" @click="fillChartData_days" />
        </SectionTitleLineWithButton>

        <CardBox class="mb-6">
          <div v-if="chartData">
            <line-chart :data="chartData" class="h-96" />
          </div>
        </CardBox>

        <SectionTitleLineWithButton :icon="mdiChartPie" title="Trends overview - ?????? 9???">
          <BaseButton :icon="mdiReload" color="whiteDark" @click="fillChartData_months" />
        </SectionTitleLineWithButton>

        <CardBox class="mb-6">
          <div v-if="chartData">
            <line-chart :data="chartData2" class="h-96" />
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
    </div>
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
  beforeMount() {
    var url = 'http://localhost:3001/api/session';
    axios.get(url, { withCredentials: true })
      .then(function (response) {
        // ?????? ?????????
        console.log(response);
        if (response.data == "") {
          window.location.href = '/login'
        }
      })
      .catch(function (error) {
        // ?????? ?????????
        console.log(error);
      })
      .then(function () {
        // ?????? ???????????? ??????
      });

    var url2 = 'http://localhost:3001/api/profile';
    axios.get(url2, { withCredentials: true })
      .then(function (response) {
        // ?????? ?????????
        console.log(response.data[0]);
        var user_name = response.data[0].user_name;

        useMainStore().setUser({
          name: user_name,
          email: "john@example.com",
          avatar:
            "https://avatars.dicebear.com/api/avataaars/example.svg?options[top][]=shortHair&options[accessoriesChance]=93",
        });

      })
      .catch(function (error) {
        // ?????? ?????????
        console.log(error);
      })
      .then(function () {
        // ?????? ???????????? ??????
      });

  },
  data() {
    return {

      is_logout: false,
      profile: []
    }
  },
  methods: {
    test: function () {
      console.log(1);
    },
    change: function () {
      console.log(1);
      window.location.href = '/login';
    },




  },


}
</script>
