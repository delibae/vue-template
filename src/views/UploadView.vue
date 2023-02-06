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

//custom modal
// import CardBoxModal from "@/components/CardBoxModal.vue";
import CardBoxModal from "@/components/CardBoxModalCustom.vue";

const selectOptions = [
  { id: 1, label: "Business development" },
  { id: 2, label: "Marketing" },
  { id: 3, label: "Sales" },
];

const form = reactive({
  name: "John Doe",
  email: "john.doe@example.com",
  phone: "",
  department: selectOptions[0],
});

const addinfo = reactive ({
  foodtype: "",
  foodamount: "100",
});

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
  formStatusCurrent.value = formStatusOptions[formStatusCurrent.value + 1]
    ? formStatusCurrent.value + 1
    : 0;
};

//추가 모달창
const modalUploadActive = ref(false)
const modalDeleteActive = ref(false)

</script>

<template>
  <LayoutAuthenticated>

    <CardBoxModal
      v-model="modalUploadActive"
      title="새로운 식사 업로드"
    >
      <p>정보를 입력하고 사진을 업로드하는 모달창</p>

      <FormField label="무슨 음식을 드셨나요?">
        <FormControl
          v-model="addinfo.foodname"
          :icon-left="mdiAccount"
          help="CurrentWeight"
          placeholder="음식명 하나를 입력하세요"
        />
      </FormField>

      <FormField label="음식을 얼마나 드셨나요?">
        <FormControl
          v-model="addinfo.foodamount"
          :icon-left="mdiAccount"
          help="CurrentWeight"
          placeholder="음식명 하나를 입력하세요"
          type="number"
        />
      </FormField>

      <BaseDivider />

      <BaseButtons>
        <BaseButton class="w-3/12" type="submit" color="info" label="추가" @click="addRow()"/>
        <BaseButton class="w-3/12" type="submit" color="info" outline label="취소" @click="modalUploadActive=false"/>
      </BaseButtons>
    </CardBoxModal>

    <CardBoxModal
      v-model="modalDeleteActive"
      title="식사를 삭제하시겠습니까?"
      button="danger"
      >
      <p>가장 마지막에 추가한 식사를 <b>삭제</b>합니다.</p>
      <p>이 작업은 <i>되돌릴 수 없습니다.</i></p>
      <BaseButtons>
        <BaseButton class="w-3/12" type="submit" color="info" label="삭제" @click="deleteRow()"/>
        <BaseButton class="w-3/12" type="submit" color="info" outline label="취소" @click="modalDeleteActive=false"/>
      </BaseButtons>
    </CardBoxModal>

    <SectionMain>
      <SectionTitleLineWithButton
        :icon="mdiBallotOutline"
        title="식단 추천 & 인증"
        main
      >
      </SectionTitleLineWithButton>
    </SectionMain>

    <SectionMain>
      <CardBox>
        <BaseDivider />

        <FormField label="Radio">
          <FormCheckRadioGroup
            v-model="customElementsForm.radio"
            name="sample-radio"
            type="radio"
            :options="{ one: 'One', two: 'Two' }"
          />
        </FormField>

      </CardBox>

      <!-- class="md:w-7/12 lg:w-5/12 xl:w-4/12 shadow-2xl md:mx-auto" -->
      <CardBox
        class="md:w-12/12 lg:w-12/12 xl:w-12/12 shadow-2xl md:mx-auto"
        is-form
        is-hoverable
        @submit.prevent="formStatusSubmit"
      >
        <NotificationBarInCard
          :color="formStatusOptions[formStatusCurrent]"
          :is-placed-with-header="formStatusWithHeader"
        >
          <span
            ><b class="capitalize">{{
              formStatusOptions[formStatusCurrent]
            }}</b>
            state</span
          >
        </NotificationBarInCard>

        <table id="fruits">
          <thead>
            <tr>
              <th>업로드 시간</th>
              <th>식사 종류</th>
              <th>식사량(g)</th>
              <th>이미지 파일</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>

        <template #footer>
          <BaseButtons>
            <!-- @click="addRow()" -->
            <BaseButton label="식사 추가" color="info" @click="modalUploadActive=true"/>
            <BaseButton label="식사 삭제" color="info" outline @click="modalDeleteActive = true"/>
          </BaseButtons>
        </template>

      </CardBox>
    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>

export default {
    methods: {
        addRow: function () {
            //현재 날짜 불러오기
            const today = new Date();
            const datetime = new Date().getFullYear() + "-" + (today.getMonth() + 1) + "-" + today.getDate() + " " + today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            // table element 중 tbody 부분 찾기
            const table = document.getElementById("fruits").getElementsByTagName("tbody")[0];
            // 새 행(Row) 추가
            const newRow = table.insertRow();
            // 새 행(Row)에 Cell 추가
            const colOne = newRow.insertCell(0);
            const colTwo = newRow.insertCell(1);
            const colThree = newRow.insertCell(2);
            const colFour = newRow.insertCell(3);
            // Cell에 텍스트 추가
            colOne.innerText = datetime;
            //new Date().toLocaleDateString();
            colTwo.innerText = "식단";
            colThree.innerText = "양";
            colFour.innerText = "이미지 파일 추가할 곳"
        },
        deleteRow: function () {
            const table = document.getElementById("fruits").getElementsByTagName("tbody")[0];
            table.deleteRow([-1]);
        },
    },
    components: { CardBoxModal }
};
</script>>