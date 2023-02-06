<script setup>
import {
    mdiMonitorCellphone,
    mdiTableBorder,
    mdiTableOff,
    mdiGithub,
    mdiTable,
    mdiCalendarTextOutline,
    mdiReload
} from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import NotificationBar from "@/components/NotificationBar.vue";
import TableSam from "@/components/TableSam.vue";
import CardBox from "@/components/CardBox.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
import BaseButton from "@/components/BaseButton.vue";
import CardBoxComponentEmpty from "@/components/CardBoxComponentEmpty.vue";
import CardBoxWidget from "@/components/CardBoxWidget.vue";
</script>

<template>
  <LayoutAuthenticated @change = "change">
        <SectionMain>

            <SectionTitleLineWithButton :icon="mdiCalendarTextOutline" title="설정 기간 분석">
                <BaseButton :icon="mdiReload" color="whiteDark" />
            </SectionTitleLineWithButton>
            <div date-rangepicker class="flex items-center">
                <div class="relative">
                    <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                        <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="currentColor"
                            viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd"
                                d="M6 2a1 1 0 00-1 1v1H4a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2h-1V3a1 1 0 10-2 0v1H7V3a1 1 0 00-1-1zm0 5a1 1 0 000 2h8a1 1 0 100-2H6z"
                                clip-rule="evenodd"></path>
                        </svg>
                    </div>
                    <input name="start" type="date"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full pl-10 p-2.5  dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Select date start">
                </div>
                <span class="mx-4 text-gray-500">~</span>
                <div class="relative">
                    <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                        <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="currentColor"
                            viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd"
                                d="M6 2a1 1 0 00-1 1v1H4a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2h-1V3a1 1 0 10-2 0v1H7V3a1 1 0 00-1-1zm0 5a1 1 0 000 2h8a1 1 0 100-2H6z"
                                clip-rule="evenodd"></path>
                        </svg>
                    </div>
                    <input name="end" type="date"
                        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full pl-10 p-2.5  dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Select date end">
                </div>
            </div>

            <br />
            <CardBox class="mb-6" has-table>
                <TableSam checkable />
            </CardBox>

            <SectionTitleLineWithButton :icon="mdiCalendarTextOutline" title="설정 기간 통계">
                <BaseButton :icon="mdiReload" color="whiteDark" />
            </SectionTitleLineWithButton>

            <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 mb-6">
                <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire" :number="avg_weight"
                    suffix="kg" label="평균 몸무게" />
                <CardBoxWidget trend="12%" trend-type="down" color="text-black-500" :icon="mdiWeightKilogram"
                    :number="avg_lose" suffix="kcal" label="평균 감량 열량" />
                <CardBoxWidget trend="Overflow" trend-type="alert" color="text-red-500" :icon="mdiFire"
                    :number="avg_consume" suffix="kcal" label="평균 소비 열량" />
            </div>

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
  beforeMount() {
        var url = 'http://localhost:3001/api/session';
        axios.get(url, { withCredentials: true })
            .then(function (response) {
                // 성공 핸들링
                console.log(response);
                if(response.data == ""){
                    window.location.href = '/login'
                }
            })
            .catch(function (error) {
                // 에러 핸들링
                console.log(error);
            })
            .then(function () {
                // 항상 실행되는 영역
            });
        
    },
  data() {
    return {
        avg_weight: 73,
        avg_lose: 2000,
        avg_consume: 3000
    }
  },
  methods: {
    test: function () {
      console.log(1);
    },
    change: function(){
      console.log(1);
      window.location.href = '/login';
    }

  },

}
</script>
