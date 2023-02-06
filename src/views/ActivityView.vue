<script setup>
import { reactive, ref } from "vue";
import { mdiBallotOutline, mdiAccount, mdiMail, mdiGithub, mdiRun, mdiPencil } from "@mdi/js";
import SectionMain from "@/components/SectionMain.vue";
import CardBox from "@/components/CardBox.vue";
import CardBoxWidget from "@/components/CardBoxWidget.vue";

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

const selectOptions = [
  { id: 1, label: "러닝" },
  { id: 2, label: "축구" },
  { id: 3, label: "수영" },
];

const form = reactive({
  working_out_code2: '',
  working_time: ''
});

const customElementsForm = reactive({
  checkbox: ["lorem"],
  radio: "one",
  switch: ["one"],
  file: null,
});

const submit = () => {
  //
};

const formStatusWithHeader = ref(true);

const formStatusCurrent = ref(0);

const formStatusOptions = ["info", "success", "danger", "warning"];

const formStatusSubmit = () => {
  // formStatusCurrent.value = formStatusOptions[formStatusCurrent.value + 1]
  //   ? formStatusCurrent.value + 1
  //   : 0;
  console.log(1);
};
</script>

<template>
  <LayoutAuthenticated @change = "change">
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiRun" title="Today Activity">
        <BaseButton :icon="mdiReload" color="whiteDark" />
      </SectionTitleLineWithButton>

      <div class="grid grid-cols-1 gap-6 lg:grid-cols-2 mb-6">
        <CardBoxWidget trend="12%" trend-type="up" color="text-red-500" :icon="mdiFire" :number="today_activity"
          suffix="kcal" label="금일 운동 열량" />
        <CardBoxWidget trend="12%" trend-type="down" color="text-red-500" :icon="mdiFire"
          :number="today_activity_goal" suffix="kcal" label="목표 운동량" />
      </div>

      <SectionTitleLineWithButton :icon="mdiPencil" title="Today Activity">
        <BaseButton :icon="mdiReload" color="whiteDark" />
      </SectionTitleLineWithButton>

      <CardBox
        class="md:w-7/12 lg:w-5/12 xl:w-6/12 shadow-2xl md:mx-auto"
        is-form
        is-hoverable
        @submit.prevent="formStatusSubmit"
      >
        <NotificationBarInCard
          :color="formStatusOptions[formStatusCurrent]"
          :is-placed-with-header="formStatusWithHeader"
        >
          <span
            ><b class="capitalize">
              운동 기록하기</b>
            </span
          >
        </NotificationBarInCard>
        <FormField label="운동 종류">
          <FormControl
            v-model="form.working_out_code2"
            :icon-left="mdiAccount"
            help="운동 종류를 입력해주세요"
            placeholder="ex. 러닝"
            :options="selectOptions"

          />
        </FormField>
        <FormField label="운동 시간">
          <FormControl
            v-model="form.working_time"
            :icon-left="mdiAccount"
            help="운동 시간을 입력해주세요"
            placeholder="ex. 2"
          />
        </FormField>
      <div class=" w-full flex flex-row justify-center">
          <BaseButton class=" w-3/12" label="submit" type="submit" color="info" />
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

