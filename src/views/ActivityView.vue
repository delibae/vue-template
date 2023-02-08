<script setup>
import { reactive, ref } from "vue";
import { mdiBallotOutline, mdiAccount, mdiMail, mdiGithub, mdiRun, mdiPencil } from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import CardBox from "@/components/CardBox.vue";
import CardBoxWidget from "@/components/CardBoxWidget.vue";
import { useMainStore } from "@/stores/main";
import FormCheckRadioGroup from "@/components/FormCheckRadioGroup.vue";
import FormFilePicker from "@/components/FormFilePicker.vue";
import FormField from "@/components/FormField.vue";
import FormControl from "@/components/FormControl.vue";
import BaseDivider from "@/components/BaseDivider.vue";
import BaseButton from "@/components/BaseButton.vue";
import BaseButtons from "@/components/BaseButtons.vue";
import SectionTitle from "@/components/SectionTitle.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
import NotificationBarInCard from "@/components/NotificationBarInCard.vue";
// to do 
let now = new Date("2022-09-01").toISOString().split("T")[0];;

const selectOptions = [
  { id: 0, label: "걷기" },
  { id: 1, label: "러닝" },
  { id: 2, label: "줄넘기" },
  { id: 3, label: "수영" },
  { id: 4, label: "자전거" },
  { id: 5, label: "에어로빅 및 필라테스" },
];

const form = reactive({
  working_out_code2: '',
  working_time: ''
});

const submit = async () => {
  console.log("here");
  var port = window.location.host;
  var protocol = window.location.protocol;
  // var url = protocol + "//" + port + '/api/predict_all';
  var url = 'http://localhost:3001/api/insertActivity';
  var frm = new FormData();
  var f_list = ['working_out_code2', 'working_time'];
  frm.append(f_list[0], form[f_list[0]].id);
  frm.append(f_list[1], form[f_list[1]]);
  frm.append('date', now)

  try {
    var response = await axios.post(url, frm, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }, withCredentials: true,
    });
    console.log("response >> ", response)
  } catch (err) {
    console.log("Error >>", err);
  }
};


const customElementsForm = reactive({
  checkbox: ["lorem"],
  radio: "one",
  switch: ["one"],
  file: null,
});


const formStatusWithHeader = ref(true);

const formStatusCurrent = ref(0);

const formStatusOptions = ["info", "success", "danger", "warning"];

const formStatusSubmit = () => {
  // formStatusCurrent.value = formStatusOptions[formStatusCurrent.value + 1]
  //   ? formStatusCurrent.value + 1
  //   : 0;
  console.log(1);
};



const today_sum = ref(null);
const activity_sum = ref(null);
// const nutrient_sum = ref(null);
// const weight_trend = ref(null);
// const nutrient_trend = ref(null);
// const yes_nutrient_sum = ref(null);
const activity_warn = ref(null);




activity_sum.value = { 'today_activity': 1000, 'today_activity_goal': 500 };

activity_warn.value = ['Good!', 'success'];

const get_today = async () => {
  try {
    var response = await axios.get('http://localhost:3001/api/profile', {
      withCredentials: true,
    });

    var current_weight = response.data[0].current_weight;

  } catch (err) {
    console.log("Error >>", err);
  }

  try {
    var response = await axios.get("http://localhost:3001/api/today/total", {
      params: { date: now },
      withCredentials: true,
    });

    var total_eat = 0;
    console.log(response.data);
    for (var i = 0; i < response.data[0].length; i++) {

      total_eat += response.data[0][i]['kcal'];
    }

    var yes_total_eat = 0;
    for (var i = 0; i < response.data[1].length; i++) {

      yes_total_eat += response.data[0][i]['kcal'];
    }

    var activity_in = { 'today_activity': 0, 'today_activity_goal': 0 };

    for (var i = 0; i < response.data[3].length; i++) {
      activity_in['today_activity'] += Math.round(response.data[3][i]['working_time'] * response.data[3][i]['coefficient']*current_weight/15);
    }

    activity_sum.value = activity_in;

    // to do 운동으로 인한 섭취 가능 칼로리 증가 필요(완료)
    today_sum.value['today_remain'] = today_sum.value['today_cal'] - total_eat + activity_in['today_activity'];

    if (today_sum.value['today_remain'] >= 0) {
      activity_sum.value['today_activity_goal'] = 0;
    } else {
      activity_sum.value['todya_activity_goal'] -= today_sum.value['total_remain'];
      activity_warn.value[0] = "Warning!";
      activity_warn.value[1] = "alert";
    }

    console.log("today total >>", response.data[3]);
  } catch (err) {
    console.log("Error >>", err);
  }
};

get_today();

</script>

<template>
  <LayoutAuthenticated @change="change">
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiRun" title="금일 활동">
        <BaseButton :icon="mdiReload" color="whiteDark" />
      </SectionTitleLineWithButton>

      <div class="grid grid-cols-1 gap-6 lg:grid-cols-2 mb-6">
        <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire"
          :number="activity_sum['today_activity']" suffix="kcal" label="금일 운동 열량" />
        <CardBoxWidget :trend="activity_warn[0]" :trend-type="activity_warn[1]" color="text-red-500" :icon="mdiFire"
          :number="activity_sum['today_activity_goal']" suffix="kcal" label="필수 운동량" />
      </div>

      <SectionTitleLineWithButton :icon="mdiPencil" title="운동 기록">
        <BaseButton :icon="mdiReload" color="whiteDark" />
      </SectionTitleLineWithButton>

      <CardBox class="md:w-7/12 lg:w-5/12 xl:w-6/12 shadow-2xl md:mx-auto" is-form is-hoverable
        @submit.prevent="formStatusSubmit">
        <NotificationBarInCard :color="formStatusOptions[formStatusCurrent]"
          :is-placed-with-header="formStatusWithHeader">
          <span><b class="capitalize">
              운동 기록하기</b>
          </span>
        </NotificationBarInCard>
        <FormField label="운동 종류">
          <FormControl v-model="form.working_out_code2" :icon-left="mdiAccount" help="운동 종류를 입력해주세요"
            placeholder="ex. 러닝" :options="selectOptions" />
        </FormField>
        <FormField label="운동 시간">
          <FormControl v-model="form.working_time" :icon-left="mdiAccount" help="운동 시간을 입력해주세요" placeholder="ex. 2" />
        </FormField>
        <div class=" w-full flex flex-row justify-center">
          <BaseButton class=" w-3/12" label="submit" type="submit" color="info" @click="submit" />
        </div>
      </CardBox>
    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>
import axios from "axios";
export default {


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

    var url2 = 'http://localhost:3001/api/profile';
    axios.get(url2, { withCredentials: true })
      .then(function (response) {
        // 성공 핸들링
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
        // 에러 핸들링
        console.log(error);
      })
      .then(function () {
        // 항상 실행되는 영역
      });

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

