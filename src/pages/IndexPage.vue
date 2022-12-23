<template>
  <q-page class="q-mb-lg">
    <div class="flex justify-center">
      <div class="q-pa-md col-xs-12">
        <h1>Compound Interest Calculator</h1>
        <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
          <q-input filled v-model="compoundForm.name" label="Your name *" hint="Name and surname" lazy-rules
            :rules="[val => val && val.length > 0 || 'Please type something']" />

          <q-input filled type="number" v-model="compoundForm.principal" label="Your Principal *" lazy-rules :rules="[
            val => val !== null && val !== '' || 'Please type your principal',
            val => val > 0 || 'Please type a real principal'
          ]" />
          <q-input filled type="number" v-model="compoundForm.regularInvestment" label="Your Regular Investment *"
            lazy-rules :rules="[
              val => val !== null && val !== '' || 'Please type your regular investment',
              val => val > 0 || 'Please type a real regular investment'
            ]" />
          <q-select filled v-model="compoundForm.regularInvestmentFrequency"
            :options="regularInvestmentFrequencyOptions" label="Select Regular Investment Frequency"
            lazy-rules :rules="[
            val => val !== null && val !== '' || 'Please select Regular Investment Frequency']" />
          <q-select filled v-model="compoundForm.compoundFrequency" :options="compoundFrequencyOptions"
            label="Select Compund Frequency" :rules="[
            val => val !== null && val !== '' || 'Please select Compound Frequency']" />
          <q-input filled type="number" v-model="compoundForm.interestRate" label="Your Interest Rate *" lazy-rules
            :rules="[
              val => val !== null && val !== '' || 'Please type your Interest Rate',
              val => val > 0 && val <= 100 || 'Interest Rate should be between 0 and 100'
            ]" />
          <q-input filled type="number" v-model="compoundForm.timeInvested" label="Your Time Invested *" lazy-rules
            :rules="[
              val => val !== null && val !== '' || 'Please type your Time Invested',
              val => val > 0 || 'Please type a real Time Invested'
            ]" />

          <div>
            <q-btn label="Submit" type="submit" color="primary" />
            <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
          </div>
        </q-form>
      </div>
    </div>
    <section class="q-mt-lg q-pa-md" id="summarySection">
      <h5 class="text-center q-animate--scale text-grey-7">{{ compoundForm.name?compoundForm.name[0].toUpperCase() + compoundForm.name.slice(1)+`'s`:'' }} Total Savings and Total Deposits</h5>
      <div class="row text-center justify-center q-gutter-lg">
        <q-card class="my-card col col-sm-4 col-xs-12 bg-orange">
          <q-card-section>
            <div class="text-h6">Initial Deposit</div>
            <div class="text-subtitle2"></div>
          </q-card-section>
          <q-separator inset />
          <q-card-section class="text-weight-light text-h5">
            {{ compoundForm.principal?compoundForm.principal:0 }}
          </q-card-section>
        </q-card>

        <q-card class="my-card col col-sm-4 col-xs-12 bg-amber">
          <q-card-section>
            <div class="text-h6">Total Additional Deposit</div>
            <div class="text-subtitle2"></div>
          </q-card-section>
          <q-separator inset />
          <q-card-section class="text-weight-light text-h5">
            {{ totalInvestment ? totalInvestment : 0 }}
          </q-card-section>
        </q-card>

        <q-card class="my-card col col-sm-4 col-xs-12 bg-lime">
          <q-card-section>
            <div class="text-h6">Total Interest</div>
            <div class="text-subtitle2"></div>
          </q-card-section>
          <q-separator inset />
          <q-card-section class="text-weight-light text-h5">
            {{ totalInterest ? totalInterest : 0 }}
          </q-card-section>
        </q-card>

        <q-card class="my-card col col-sm-4 col-xs-12 bg-light-green">
          <q-card-section>
            <div class="text-h6">Total Savings</div>
            <div class="text-subtitle2"></div>
          </q-card-section>
          <q-separator inset />
          <q-card-section class=" text-h5">
            {{ futureValue ? futureValue : 0 }}
          </q-card-section>
        </q-card>
      </div>
    </section>
    <div class="q-mt-lg q-pa-sm row items-start">
      <q-card class="my-card col-md-10 col-xs-12 q-mx-auto">

        <q-card-section>
          <div class="text-h6">Interest Plan Chart</div>
          <!-- <div class="text-subtitle2">by Hari</div> -->
        </q-card-section>
        <!-- <img src="https://cdn.quasar.dev/img/mountains.jpg" style="height:250px"> -->
        <div class="q-py-md q-px-xs" id="InterestChart">
          <BarChart v-if="isdataCalculated" :chartData="datacollection.data" :chartOptions="chartOptions" />

          <div class="chart" v-else>
            <div class="no-chart-data">
              <h2 class="title">No data</h2>
              <div class="y-axis"></div>
              <div class="x-axis"></div>
              <div class="bars">
                <div class="overlay"></div>
                <div class="bar bar-0"></div>
                <div class="bar bar-1"></div>
                <div class="bar bar-2"></div>
                <div class="bar bar-3"></div>
                <div class="bar bar-4"></div>
                <div class="bar bar-5"></div>
                <div class="bar bar-6"></div>
                <div class="bar bar-7"></div>
                <div class="bar bar-8"></div>
                <div class="bar bar-9"></div>
                <div class="bar bar-10"></div>
                <div class="bar bar-11"></div>
                <div class="bar bar-12"></div>
                <div class="bar bar-13"></div>
                <div class="bar bar-14"></div>
                <div class="bar bar-15"></div>
                <div class="bar bar-16"></div>
                <div class="bar bar-17"></div>
                <div class="bar bar-18"></div>
                <div class="bar bar-19"></div>
                <div class="bar bar-20"></div>
                <div class="bar bar-21"></div>
                <div class="bar bar-22"></div>
                <div class="bar bar-23"></div>
              </div>
            </div>
          </div>
        </div>

      </q-card>

      <div class="q-mx-auto col-md-10 col-xs-12 q-pt-lg">
        <q-table title="Yearly Interest Data Table" title-class="text-h6" :rows="tableDataRows" :columns="tableDatacolumns"  row-key="name" :pagination="{rowsPerPage:0}" no-data-label="No data available please add values in above form fields to see it here."  >

          <template v-slot:header="props">
          <q-tr :props="props">
            <q-th
              v-for="col in props.cols"
              :key="col.name"
              :props="props"
              class="text-dark-green"
            >
              {{ col.label }}
            </q-th>
          </q-tr>
          </template>

          <template v-slot:body-cell-interest="props">
            <q-td key="interest" class="text-bold" :props="props">
              <q-badge color="" class="q-px-md q-py-md text-bold bg-orange-7">
                {{ props.row.interest }}
              </q-badge>
            </q-td>
          </template>

          <template v-slot:no-data="{ icon, message, filter }">
            <div class="full-width row flex-center text-body2 q-gutter-sm">
              <q-icon size="2em" name="sentiment_dissatisfied" />
              <span>
                {{ message }}
              </span>
              <q-icon size="2em" :name="filter ? 'filter_b_and_w' : icon" />
            </div>
          </template>


        </q-table>

      </div>
    </div>
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { ref, reactive } from 'vue'
import { computed } from '@vue/reactivity';
import BarChart from 'components/BarChart.vue';


const $q = useQuasar()
const compoundForm = reactive({
  name: null,
  principal: 0,
  regularInvestment: 0,
  interestRate: 0,
  timeInvested: 0,
  compoundFrequency: null,
  regularInvestmentFrequency: null,
})
const regularInvestmentFrequencyOptions = [
  { "value": "1", "label": "Yearly", },
  { "value": "12", "label": "Monthly", },
  { "value": "26", "label": "Fortnightly", },
  { "value": "52", "label": "Weekly", },
  { "value": "365", "label": "Daily" },
];
const compoundFrequencyOptions = [
  { "value": "1", "label": "Yearly" },
  { "value": "12", "label": "Monthly" }
];

const tableDataRows = ref([]);
const tableDatacolumns = [
  { name: 'years', label: 'Years', field: 'year', sortable: true },
  { name: 'principal', align: 'center', label: 'Principal Amount', field: 'principal', sortable: true },
  { name: 'investment', align: 'left', label: 'Recurring Investment', field: 'investment', sortable: true },
  { name: 'interest', align: 'left', label: 'Interest Accumulated', field: 'interest', sortable: true },
];

let datacollection = reactive({ data: {} });
let chartOptions = reactive({responsive: true, maintainAspectRatio: false});


let isdataCalculated = computed(() => {
  return Object.keys(datacollection.data).length;
})

const onSubmit = () => {
  if (!compoundForm.principal || !compoundForm.regularInvestment || !compoundForm.interestRate || !compoundForm.timeInvested || !compoundForm.compoundFrequency || !compoundForm.regularInvestmentFrequency) {
    $q.notify({
      color: 'red-5',
      textColor: 'white',
      icon: 'warning',
      message: 'You need to input all the fields in the form to calculate compound interest.'
    })
  } else {
    caclulateCompundInterest();

    // console.log("data", datacollection);
    $q.notify({
      color: 'green-4',
      textColor: 'white',
      icon: 'cloud_done',
      message: 'Data Generated!'
    })
  }
}

const onReset = () => {
  // console.log('form', compoundForm);
  for (let i in compoundForm) {
    compoundForm[i] = null;
  }
  tableDataRows.value = [];
  datacollection.data = [];

}

const caclulateCompundInterest = () => {

  let cols = [];
  let years = 0;
  let principalAmount = [];
  let investmentsAmount = [];
  let interestAmount = [];
  tableDataRows.value = [];

  for (var i = compoundForm.timeInvested - 1; i >= 0; i--) {

    // Years Labels
    years = years + 1;
    cols.push(years + ' Years')

    // Append  principal amount
    principalAmount.push(compoundForm.principal);

    // Append Regular Investment money
    let investVal = years * compoundForm.regularInvestment * compoundForm.regularInvestmentFrequency.value;
    investmentsAmount.push(investVal);

    // Count interest and append it
    let interest = yearlyInterest(years);
    interestAmount.push(interest);

    // Append all data in rows for table
    tableDataRows.value.push({
      year: years+ " Year",
      principal: compoundForm.principal,
      investment: investVal,
      interest: interest,
    })

  }

  // Arrange data to show in chart
  datacollection.data = {
    labels: cols,
    datasets: [{
      label: 'Principal',
      backgroundColor: '#ffa8a2',
      data: principalAmount
    },
    {
      label: 'Total Investments',
      backgroundColor: '#a2ffb8',
      data: investmentsAmount
    },
    {
      label: 'Compound Interest',
      backgroundColor: '#ffab41',
      data: interestAmount
    }
    ]
  }



}

const yearlyInterest = (years) => {

  let totalInvPeriod = parseFloat(compoundForm.regularInvestmentFrequency.value) * (years);

  let totalRegInv = parseFloat(compoundForm.regularInvestment) * totalInvPeriod;

  let interestRate = parseFloat(compoundForm.interestRate) / 100;
  let compoundFreq = parseFloat(compoundForm.compoundFrequency.value);
  let regInvFreq = parseFloat(compoundForm.regularInvestmentFrequency.value);

  let rt = (Math.pow((1 + interestRate / compoundFreq), (compoundFreq / regInvFreq)) - 1);

  let principl = parseFloat(compoundForm.principal);
  let regInv = parseFloat(compoundForm.regularInvestment);

  let fv = ((principl * (Math.pow((1 + rt), totalInvPeriod))) + (regInv * (Math.pow((1 + rt), totalInvPeriod) - 1) / rt));
  let futureAmountValue = fv.toFixed(2);

  let totalInt = ((futureAmountValue - principl) - totalRegInv);
  return totalInt.toFixed(2);

}

// Total Investment Period
const totalInvestmentPeriods = computed(() => {
  return parseFloat(compoundForm.regularInvestmentFrequency?.value) * parseFloat(compoundForm.timeInvested);
})

// Multiply total investment period with investment amount
const totalInvestment = computed(() => {
  return parseFloat(compoundForm.regularInvestment) * totalInvestmentPeriods?.value;
})

const rate = computed(() => {

  let interestRate = parseFloat(compoundForm.interestRate) / 100;
  let compoundFreq = parseFloat(compoundForm.compoundFrequency?.value);
  let regInvFreq = parseFloat(compoundForm.regularInvestmentFrequency?.value);
  return (Math.pow((1 + interestRate / compoundFreq), (compoundFreq / regInvFreq)) - 1);
})

const futureValue = computed(() => {

  if (!compoundForm.principal) return 0;

  let principl = parseFloat(compoundForm.principal);
  let rt = parseFloat(rate?.value);
  let totalInvPeriod = parseFloat(totalInvestmentPeriods?.value);
  let regInv = parseFloat(compoundForm.regularInvestment);

  // Derive final future value
  let fv = ((principl * (Math.pow((1 + rt), totalInvPeriod))) + (regInv * (Math.pow((1 + rt), totalInvPeriod) - 1) / rt));
  return fv.toFixed(2);
})

const totalInterest = computed(() => {
  if (!compoundForm.principal) return 0;

  let fv = parseFloat(futureValue?.value);
  let principl = parseFloat(compoundForm.principal);
  let totalRegInv = parseFloat(totalInvestment?.value);
  let totalInt = ((fv - principl) - totalRegInv);
  return totalInt.toFixed(2);
})
</script>
<style script>
h1{
  font-size: xxx-large;
  line-height: normal;
}
.w-50 {
  width: 50%;
}
.bg-light-green.my-card {
  box-shadow: 0px 0px 20px 0 #4d850d;
}
.bg-lime.my-card {
  box-shadow: 0px 0px 20px 0 #828d16;;
}
.bg-amber.my-card {
  box-shadow: 0px 0px 20px 0 #8b6d14;
}
.bg-orange.my-card {
  box-shadow: 0px 0px 20px 0 #9f6004;
}
.q-table th, .q-table tbody td{
  font-size: 18px;
}
.q-table tr:last-child{
  background-color: #c1f4cd !important
}
.q-table__container {
    background-color: #f3fff6 !important;
}
.text-dark-green{
  color: green;
}
.q-badge.q-badge--single-line{
  font-size: 18px;
}
#InterestChart canvas{
  min-height: 300px;
}

.chart {
  position: relative;
  width: 100%;
  height: 300px;
  overflow: hidden;
}

.no-chart-data {
  background: #FFF;
  min-height: 300px;
  display: block;
  width: 100%;
  overflow: hidden;
}
.no-chart-data .title {
  color: #757575;
  position: absolute;
  display: flex;
  left: 50%;
  top: 50%;
  margin-right: -50%;
  transform: translate(-50%, -100%);
  z-index: 100;
}
.no-chart-data .y-axis, .no-chart-data .x-axis {
  position: absolute;
  left: 25px;
  bottom: 25px;
}
.no-chart-data .y-axis {
  height: 350px;
  width: 1px;
  border-left: 2px solid #F5F5F5;
}
.no-chart-data .x-axis {
  height: 1px;
  width: 95%;
  border-bottom: 2px solid #F5F5F5;
}
.no-chart-data .bars {
  position: absolute;
  display: inline-block;
  bottom: 15px;
  left: 35px;
  width: 100%;
  margin: 0 auto;
  background: rgba(0, 0, 0, 0.1);
}
.no-chart-data .bars .bar {
  background: #F5F5F5;
  display: inline-block;
  position: absolute;
  min-height: 5px;
  width: 3%;
  max-width: 25px;
  bottom: 10px;
  margin: 0 0.8%;
  border-top: 1px solid #FFF;
  border-left: 1px solid #FFF;
  border-right: 1px solid #FFF;
}
.no-chart-data .bars .bar.bar-0 {
  height: 259px;
  display: inline-block;
  margin-left: 1%;
}
.no-chart-data .bars .bar.bar-1 {
  height: 45px;
  display: inline-block;
  margin-left: 5%;
}
.no-chart-data .bars .bar.bar-2 {
  height: 331px;
  display: inline-block;
  margin-left: 9%;
}
.no-chart-data .bars .bar.bar-3 {
  height: 133px;
  display: inline-block;
  margin-left: 13%;
}
.no-chart-data .bars .bar.bar-4 {
  height: 195px;
  display: inline-block;
  margin-left: 17%;
}
.no-chart-data .bars .bar.bar-5 {
  height: 224px;
  display: inline-block;
  margin-left: 21%;
}
.no-chart-data .bars .bar.bar-6 {
  height: 147px;
  display: inline-block;
  margin-left: 25%;
}
.no-chart-data .bars .bar.bar-7 {
  height: 191px;
  display: inline-block;
  margin-left: 29%;
}
.no-chart-data .bars .bar.bar-8 {
  height: 252px;
  display: inline-block;
  margin-left: 33%;
}
.no-chart-data .bars .bar.bar-9 {
  height: 136px;
  display: inline-block;
  margin-left: 37%;
}
.no-chart-data .bars .bar.bar-10 {
  height: 48px;
  display: inline-block;
  margin-left: 41%;
}
.no-chart-data .bars .bar.bar-11 {
  height: 314px;
  display: inline-block;
  margin-left: 45%;
}
.no-chart-data .bars .bar.bar-12 {
  height: 125px;
  display: inline-block;
  margin-left: 49%;
}
.no-chart-data .bars .bar.bar-13 {
  height: 95px;
  display: inline-block;
  margin-left: 53%;
}
.no-chart-data .bars .bar.bar-14 {
  height: 271px;
  display: inline-block;
  margin-left: 57%;
}
.no-chart-data .bars .bar.bar-15 {
  height: 359px;
  display: inline-block;
  margin-left: 61%;
}
.no-chart-data .bars .bar.bar-16 {
  height: 346px;
  display: inline-block;
  margin-left: 65%;
}
.no-chart-data .bars .bar.bar-17 {
  height: 11px;
  display: inline-block;
  margin-left: 69%;
}
.no-chart-data .bars .bar.bar-18 {
  height: 223px;
  display: inline-block;
  margin-left: 73%;
}
.no-chart-data .bars .bar.bar-19 {
  height: 153px;
  display: inline-block;
  margin-left: 77%;
}
.no-chart-data .bars .bar.bar-20 {
  height: 246px;
  display: inline-block;
  margin-left: 81%;
}
.no-chart-data .bars .bar.bar-21 {
  height: 346px;
  display: inline-block;
  margin-left: 85%;
}
.no-chart-data .bars .bar.bar-22 {
  height: 260px;
  display: inline-block;
  margin-left: 89%;
}
</style>
