<script setup>
import { reactive, ref, computed, onMounted } from "vue";
import { useMainStore } from "@/stores/main";
import {
  mdiBallotOutline,
  mdiAccount,
  mdiMail,
  mdiGithub,
  mdiFire,
  mdiRice,
  mdiFoodDrumstick,
  mdiPigVariantOutline,
} from "@mdi/js";
import { get_days, get_months } from "@/rest/chartconfig.js";
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
import CardBoxWidget from "@/components/CardBoxWidget.vue";

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

const addinfo = reactive({
  foodtype: "",
  foodamount: "",
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

//????????? (??????)
const modalInputActive = ref(false); //??????????????? ??????
const modalDetectionActive = ref(false); //???????????? ???????????? ??????
const modalDeleteActive = ref(false); //??????

//Home?????? ??????
let now = new Date("2022-09-01");

const chartData = ref(null);

const chartData2 = ref(null);

const fillChartData_days = async () => {
  var rrr = await get_days();

  chartData.value = rrr;
};

const fillChartData_months = async () => {
  var rrr = await get_months();

  chartData2.value = rrr;
};

const today_sum = ref(null);
const activity_sum = ref(null);
const nutrient_sum = ref(null);
const weight_trend = ref(null);
const nutrient_trend = ref(null);
const yes_nutrient_sum = ref(null);
const activity_warn = ref(null);

weight_trend.value = ["13%", "down"];
today_sum.value = { today_cal: 3000, today_weight: 70, today_remain: 1000 };
nutrient_sum.value = { fat: 300, protein: 400, carbo: 300 };
yes_nutrient_sum.value = { fat: 300, protein: 400, carbo: 300 };
nutrient_trend.value = {
  fat: ["12%", "down"],
  protein: ["13%", "up"],
  carbo: ["0%", "mid"],
};

activity_sum.value = { today_activity: 1000, today_activity_goal: 500 };

activity_warn.value = ["Good!", "success"];

const get_today = async () => {
  try {
    var response = await axios.get("http://localhost:3001/api/profile", {
      withCredentials: true,
    });
    today_sum.value["today_cal"] = response.data[0].kcal_per_day;
    today_sum.value["today_weight"] = response.data[0].current_weight;
    let now = new Date("2022-09-01");
  } catch (err) {
    console.log("Error >>", err);
  }

  try {
    var response = await axios.get("http://localhost:3001/api/today/total", {
      params: { date: now },
      withCredentials: true,
    });
    var n_in = { fat: 0, protein: 0, carbo: 0 };
    var total_eat = 0;
    console.log(response.data);
    for (var i = 0; i < response.data[0].length; i++) {
      n_in["fat"] += response.data[0][i]["fat"];
      n_in["protein"] += response.data[0][i]["protein"];
      n_in["carbo"] += response.data[0][i]["carbo"];
      total_eat += response.data[0][i]["kcal"];
    }
    nutrient_sum.value = n_in;
    var n_in = { fat: 0, protein: 0, carbo: 0 };
    var yes_total_eat = 0;
    for (var i = 0; i < response.data[1].length; i++) {
      n_in["fat"] += response.data[1][i]["fat"];
      n_in["protein"] += response.data[1][i]["protein"];
      n_in["carbo"] += response.data[1][i]["carbo"];
      yes_total_eat += response.data[0][i]["kcal"];
    }
    yes_nutrient_sum.value = n_in;
    console.log("yes", yes_nutrient_sum.value);

    var label_list = ["fat", "protein", "carbo"];
    for (var i = 0; i < label_list.length; i++) {
      var percent = Math.ceil(
        ((nutrient_sum.value[label_list[i]] -
          yes_nutrient_sum.value[label_list[i]]) *
          100) /
          yes_nutrient_sum.value[label_list[i]]
      );
      nutrient_trend.value[label_list[i]][0] = percent + "%";
      if (percent > 0) {
        nutrient_trend.value[label_list[i]][1] = "up";
      } else if (percent < 0) {
        nutrient_trend.value[label_list[i]][1] = "down";
      } else {
        nutrient_trend.value[label_list[i]][1] = "mid";
      }
    }

    var yes_weight = response.data[2][0].weight;
    var w_percent = Math.ceil(
      ((today_sum.value.today_weight - yes_weight) * 100) / yes_weight
    );
    weight_trend.value[0] = w_percent + "%";

    if (w_percent > 0) {
      weight_trend.value[1] = "up";
    } else if (w_percent < 0) {
      weight_trend.value[1] = "down";
    } else {
      weight_trend.value[1] = "mid";
    }

    var activity_in = { today_activity: 0, today_activity_goal: 0 };

    for (var i = 0; i < response.data[3].length; i++) {
      activity_in["today_activity"] +=
        response.data[3][i]["working_time"] *
        response.data[3][i]["coefficient"];
    }

    activity_sum.value = activity_in;

    // to do ???????????? ?????? ?????? ?????? ????????? ?????? ??????(??????)
    today_sum.value["today_remain"] =
      today_sum.value["today_cal"] - total_eat + activity_in["today_activity"];

    if (today_sum.value["today_remain"] >= 0) {
      activity_sum.value["today_activity_goal"] = 0;
    } else {
      activity_sum.value["todya_activity_goal"] -=
        today_sum.value["total_remain"];
      activity_warn.value[0] = "Warning!";
      activity_warn.value[1] = "alert";
    }

    console.log("today total >>", response.data[3]);
  } catch (err) {
    console.log("Error >>", err);
  }
};

get_today();

const get_sum = function () {
  console.log(today_sum["today_cal"]);
};

onMounted(() => {
  fillChartData_days();
  fillChartData_months();
});

const mainStore = useMainStore();

const clientBarItems = computed(() => mainStore.clients.slice(0, 4));

const transactionBarItems = computed(() => mainStore.history);
//Home?????? ??????
</script>

<template>
  <LayoutAuthenticated>
    <CardBoxModal v-model="modalInputActive" title="????????? ?????? ??????">
      <p>?????? ????????? ??????</p>
      <FormField label="?????? ????????? ?????????????">
        <FormControl
          v-model="addinfo.foodname"
          :icon-left="mdiAccount"
          help="WhatFood"
          id="whatfood"
          placeholder="????????? ????????? ???????????????"
        />
      </FormField>

      <FormField label="????????? ????????? ?????????????">
        <FormControl
          v-model="addinfo.foodamount"
          :icon-left="mdiAccount"
          help="CurrentWeight"
          id="foodamount"
          placeholder="?????? ????????? ?????? ???????????????"
          type="number"
        />
      </FormField>

      <BaseDivider />

      <BaseButtons>
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          label="??????"
          @click="addRow()"
        />
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          outline
          label="??????"
          @click="modalInputActive = false"
        />
      </BaseButtons>
    </CardBoxModal>

    <CardBoxModal
      v-model="modalDetectionActive"
      title="??????????????? ??????????????? ??????"
    >
      <FormField label="????????? ?????????" help="????????? ????????? ????????? ?????? ??????">
        <FormFilePicker label="?????? ??????" />
      </FormField>

      <BaseButtons title>
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          label="??????"
          @click="sendImage"
        />
        <p>[?????? ???????????? ??????]</p>
      </BaseButtons>

      <BaseButtons>
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          label="??????"
          @click="addRow()"
        />
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          outline
          label="??????"
          @click="modalDetectionActive = false"
        />
      </BaseButtons>
    </CardBoxModal>

    <CardBoxModal
      v-model="modalDeleteActive"
      title="????????? ?????????????????????????"
      button="danger"
    >
      <p>?????? ???????????? ????????? ????????? <b>??????</b>?????????.</p>
      <p>??? ????????? <i>????????? ??? ????????????.</i></p>
      <BaseButtons>
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          label="??????"
          @click="deleteRow()"
        />
        <BaseButton
          class="w-3/12"
          type="submit"
          color="info"
          outline
          label="??????"
          @click="modalDeleteActive = false"
        />
      </BaseButtons>
    </CardBoxModal>

    <SectionMain>
      <SectionTitleLineWithButton
        :icon="mdiBallotOutline"
        title="?????? ?????? & ??????"
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
          <span>?????? ??????</span>
        </NotificationBarInCard>

        <table id="fruits">
          <thead>
            <tr>
              <th>????????? ??????</th>
              <th>?????? ??????</th>
              <th>?????????(g)</th>
              <th>??????</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>

        <template #footer>
          <BaseButtons>
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalInputActive = true"
            />
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalDetectionActive = true"
            />
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalDeleteActive = true"
            />
          </BaseButtons>
        </template>
      </CardBox>

      <SectionMain>
        <div class="grid grid-cols-1 gap-6 lg:grid-cols-4 mb-6">
          <CardBoxWidget
            class="shadow-2xl"
            trend="Info"
            trend-type="success"
            color="text-red-500"
            :icon="mdiFire"
            :number="today_sum['today_remain']"
            suffix="kcal"
            label="?????? ?????? ?????? ??????"
          />
          <CardBoxWidget
            class="shadow-2xl"
            :trend="nutrient_trend['carbo'][0]"
            :trend-type="nutrient_trend['carbo'][1]"
            color="text-lime-400"
            :icon="mdiRice"
            :number="nutrient_sum['carbo']"
            suffix="g"
            label="?????? ????????????"
          />
          <CardBoxWidget
            class="shadow-2xl"
            :trend="nutrient_trend['protein'][0]"
            :trend-type="nutrient_trend['protein'][1]"
            color="text-orange-800"
            :icon="mdiFoodDrumstick"
            :number="nutrient_sum['protein']"
            suffix="g"
            label="?????? ?????????"
          />
          <CardBoxWidget
            class="shadow-2xl"
            :trend="nutrient_trend['fat'][0]"
            :trend-type="nutrient_trend['fat'][1]"
            color="text-rose-300"
            :icon="mdiPigVariantOutline"
            :number="nutrient_sum['fat']"
            suffix="g"
            label="?????? ??????"
          />
        </div>
      </SectionMain>

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
          <span>?????? ??????</span>
        </NotificationBarInCard>
        
        <div id="app">
          <table>
            <thead>
              <th v-for="item in header" :key="item">{{ item }}</th>
            </thead>
            <tbody>
              <tr v-for="line in ranking" :key="line">
                <td v-for="item in line" :key="item">{{ item }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <template #footer>
          <BaseButtons>
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalInputActive = true"
            />
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalDetectionActive = true"
            />
            <BaseButton
              label="?????? ??????"
              color="info"
              @click="modalDeleteActive = true"
            />
          </BaseButtons>
        </template>
      </CardBox>
    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>
import axios from "axios";
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
  methods: {
    addRow: function () {
      //?????? ?????? ????????????
      const today = new Date();
      const datetime =
        new Date().getFullYear() +
        "-" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate() +
        " " +
        today.getHours() +
        ":" +
        today.getMinutes() +
        ":" +
        today.getSeconds();
      // table element ??? tbody ?????? ??????
      const table = document
        .getElementById("fruits")
        .getElementsByTagName("tbody")[0];
      // ??? ???(Row) ??????
      const newRow = table.insertRow();
      // ??? ???(Row)??? Cell ??????
      const colOne = newRow.insertCell(0);
      const colTwo = newRow.insertCell(1);
      const colThree = newRow.insertCell(2);
      const colFour = newRow.insertCell(3);
      // Cell??? ????????? ??????
      colOne.innerText = datetime;
      //new Date().toLocaleDateString();
      colTwo.innerText = document.getElementById("whatfood").value;
      colThree.innerText = document.getElementById("foodamount").value;
      colFour.innerHTML = '<input type="submit" value="??????">'
      //?????? ?????? ?????? ?????? ?????? ??????
      //?????? ??? ?????????
      document.getElementById("whatfood").value="";
      document.getElementById("foodamount").value="";
    },
    deleteRow: function () {
      const table = document
        .getElementById("fruits")
        .getElementsByTagName("tbody")[0];
      table.deleteRow([-1]);
    },

    test: function () {
      console.log(1);
    },
    change: function () {
      console.log(1);
      window.location.href = "/login";
    },
  },
  components: { CardBoxModal },

  name: "LoginView",
  components: {},
  beforeMount() {
    var url = "http://localhost:3001/api/session";
    axios
      .get(url, { withCredentials: true })
      .then(function (response) {
        // ?????? ?????????
        console.log(response);
        if (response.data == "") {
          window.location.href = "/login";
        }
      })
      .catch(function (error) {
        // ?????? ?????????
        console.log(error);
      })
      .then(function () {
        // ?????? ???????????? ??????
      });

    var url2 = "http://localhost:3001/api/profile";
    axios
      .get(url2, { withCredentials: true })
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
      profile: [],
      header: [
        "??????",
        "????????????(%)",
        "?????????(%)",
        "??????(%)",
        "?????????(%)",
        "?????????(g)",
      ],
      ranking: [
        ["????????????", "10", "10", "10", "10", "200"],
        ["????????????", "10", "10", "10", "10", "200"],
        ["????????????", "10", "10", "10", "10", "200"],
        ["????????????", "10", "10", "10", "10", "200"],
      ],
    };
  },
};
</script>
<style>
table {
  width: 75%;
  text-align: left;
}
table th {
  padding: 12px;
  border-bottom: 2px solid darkgray;
}
table td {
  padding: 12px;
}
table tr:nth-of-type(even) {
  background-color: rgba(0, 0, 255, 0.1);
}
.box {
  padding: 10px;
  margin: 10px;
  background-color: rgba(0, 0, 255, 0.1);
}
</style>