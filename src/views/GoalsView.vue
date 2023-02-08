<script setup>
import { reactive, ref } from "vue";
import { mdiBallotOutline, mdiAccount, mdiMail, mdiGithub } from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import CardBox from "@/components/CardBox.vue";
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
import { useMainStore } from "@/stores/main";
// to do 
let now = new Date("2022-09-01");

const numberMeals = [
  { id: 1, label: "1끼" },
  { id: 2, label: "2끼" },
  { id: 3, label: "3끼" },
  { id: 4, label: "4끼" },
];

const form = ref(null);

form.value = {
  purpose: 0,
  d_day: new Date().toISOString().slice(0, 10),
  current_weight: "",
  goal_weight: "",
  // num_meals: numberMeals[2],  //default: 3끼
  minimum_kcal: ""
}


const get_prev = async () => {
  try {
    var response = await axios.get('http://localhost:3001/api/profile', {
      withCredentials: true,
    });
    console.log("response >>", response);
    form.value.purpose = response.data[0].purpose;
    form.value.current_weight = response.data[0].current_weight;

    form.value.d_day = new Date(response.data[0].d_day).toISOString().split("T")[0];;
    form.value.goal_weight = response.data[0].goal_weight;
    form.value.minimum_kcal = response.data[0].minimum_kcal;
    console.log("form >> ", form.value);
  } catch (err) {
    console.log("Error >>", err);
  }
}

get_prev()



const submit = async () => {

  var port = window.location.host;
  var protocol = window.location.protocol;
  // var url = protocol + "//" + port + '/api/predict_all';
  var url = 'http://localhost:3001/api/profile/updateGoal';
  var frm = new FormData();
  var f_list = ['purpose', 'current_weight', 'goal_weight','minimum_kcal','d_day'];
  for(var i = 0 ; i < f_list.length; i++){
    frm.append(f_list[i], form.value[f_list[i]]);
  }
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

const formStatusWithHeader = ref(true);

const formStatusCurrent = ref(0);

const formStatusOptions = ["info", "success", "danger", "warning"];

const formStatusSubmit = () => {
  formStatusCurrent.value = formStatusOptions[formStatusCurrent.value + 1]
    ? formStatusCurrent.value + 1
    : 0;
};
</script>

<template>
  <LayoutAuthenticated @change="change">

    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiBallotOutline" title="체중과 목표 기록" main />

      <UserCard class="mb-6" />

      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <CardBox>

          <FormField label="사용 목적">
            <FormCheckRadioGroup v-model="form.purpose" name="sample-radio" type="radio"
              :options="{ 0: '다이어트', 1: '벌크업' }" />
          </FormField>

          <!-- <FormField label="식사 횟수">
            <FormControl v-model="form.num_meals" :options="numberMeals" />
          </FormField> -->

          <FormField label="마감 날짜" help="최대 한 달 후까지 선택할 수 있습니다.">
            <FormControl v-model="form.d_day" :icon-left="mdiAccount" help="TargetDate"
              placeholder="목표 날짜 입력 [YYYYMMDD]" type="date" />
          </FormField>

        </CardBox>

        <CardBox>

          <FormField label="현재 몸무게(kg)">
            <FormControl v-model="form.current_weight" :icon-left="mdiAccount" help="CurrentWeight"
              placeholder="숫자로만 입력" type="number" />
          </FormField>

          <FormField label="목표 몸무게(kg)">
            <FormControl v-model="form.goal_weight" :icon-left="mdiAccount" help="TargetWeight" placeholder="숫자로만 입력"
              type="number" />
          </FormField>

          <FormField label="최소 섭취 칼로리(kcal)" help="하루 동안 섭취할 최소 열량입니다.">
            <FormControl v-model="form.minimum_kcal" :icon-left="mdiAccount" help="TargetKcal" placeholder="숫자로만 입력"
              type="number" />
          </FormField>

          <BaseDivider />

          <div class=" w-full flex flex-row justify-center">
            <BaseButton @click="submit" class="w-3/12" type="submit" color="info" label="저장" />
          </div>
        </CardBox>
      </div>

    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>
import axios from 'axios';

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
    },
  },
}

</script>