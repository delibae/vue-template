<script setup>
import {
    mdiMonitorCellphone,
    mdiTableBorder,
    mdiTableOff,
    mdiGithub,
    mdiTable,
    mdiCalendarTextOutline,
    mdiReload,
    mdiFire,
    mdiRice,
    mdiFoodDrumstick,
    mdiPigVariantOutline,
} from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import NotificationBar from "@/components/NotificationBar.vue";
import TableSam2 from "@/components/TableSam2.vue";
import CardBox from "@/components/CardBox.vue";
import CardBoxModal from "@/components/CardBoxModalCustom.vue";
import FormFilePicker from "@/components/FormFilePicker.vue";
import FormField from "@/components/FormField.vue";
import FormControl from "@/components/FormControl.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
import BaseButton from "@/components/BaseButton.vue";
import CardBoxComponentEmpty from "@/components/CardBoxComponentEmpty.vue";
import CardBoxWidget from "@/components/CardBoxWidget.vue";

import { ref, watch } from "vue";

const lineData = ref(null);



lineData.value = [

    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },
    {
        "date": "Dec 1, 2021",
        "meal_type": "토스트",
        "meal_amount": "200",
        "meal_cal": "300",
    },

]


const push_data = function (data) {
    console.log("data>>", data);
    var bef = lineData.value;
    bef.push(data)
    var aft = bef;
    console.log("aft>>", aft);
    for (var i = 0; i < aft.length; i++) {
        console.log("item >>", aft[i]);
    }
    lineData.value = aft;
}

</script>




<template>
    <LayoutAuthenticated @change="change">
        <SectionMain>

            <SectionTitleLineWithButton :icon="mdiCalendarTextOutline" title="식단 추천 및 인증">
                <BaseButton :icon="mdiReload" color="whiteDark" />
            </SectionTitleLineWithButton>

            <br />
            <CardBox class="mb-6" has-table>
                <TableSam2 checkable :lineData="lineData" @push_data="push_data" />
            </CardBox>

            <SectionTitleLineWithButton :icon="mdiCalendarTextOutline" title="Today Overview">
                <BaseButton :icon="mdiReload" color="whiteDark" />
            </SectionTitleLineWithButton>

            <div class="grid grid-cols-1 gap-6 lg:grid-cols-4 mb-6">
                <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire" :number="avg_weight"
                    suffix="kg" label="남은 섭취 가능 열량" />
                <CardBoxWidget trend="12%" trend-type="down" color="text-lime-500" :icon="mdiRice" :number="avg_lose"
                    suffix="kcal" label="남은 탄수화물" />
                <CardBoxWidget trend="Overflow" trend-type="mid" color="text-orange-500" :icon="mdiFoodDrumstick"
                    :number="avg_consume" suffix="kcal" label="남은 단백질" />
                <CardBoxWidget trend="Overflow" trend-type="alert" color="text-rose-500" :icon="mdiPigVariantOutline"
                    :number="avg_consume" suffix="kcal" label="남은 지방" />
            </div>

        </SectionMain>
    </LayoutAuthenticated>
</template>

<script>
import axios from 'axios';

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
                if (response.data == "") {
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
        change: function () {
            console.log(1);
            window.location.href = '/login';
        }

    },

}
</script>
