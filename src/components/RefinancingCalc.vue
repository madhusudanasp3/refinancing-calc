<template>
  <div class="calculator">
    <div class="container">
      <div class="row">
        <div class="col text-left page-header padding-left--text margin--bottom">
          <h4>{{ msg }}</h4>
        </div>
      </div>
      <nav>
        <div id="nav-tab" class="nav nav-tabs" role="tablist">
          <a
            id="input-tab"
            class="nav-item nav-link"
            data-toggle="tab"
            href="#input-tab-content"
            role="tab"
            aria-controls="input"
            aria-selected="true"
            :class="{ 'active': visibleTab === 0}"
            @click="visibleTab = 0"
          >Input</a>
          <a
            id="result-tab"
            class="nav-item nav-link"
            data-toggle="tab"
            href="#result-tab-content"
            role="tab"
            aria-controls="result"
            aria-selected="false"
            :class="[result.monthlyPayment1 == null ? 'disabled': '',
                     visibleTab === 1 ? 'active' : '']"
            @click="visibleTab = 1"
          >Result</a>
        </div>
      </nav>
      <div id="tabContent" class="tab-content">
        <div
          id="input-tab-content"
          class="tab-pane fade"
          role="tabpanel"
          aria-labelledby="input-tab"
          :class="{ 'show active': visibleTab === 0 }"
        >
          <InputComponent v-on:submit-form="displayResults"></InputComponent>
        </div>
        <div
          id="result-tab-content"
          class="tab-pane fade"
          role="tabpanel"
          aria-labelledby="result-tab"
          :class="{ 'show active': visibleTab === 1 }"
        >
          <ResultComponent v-bind:result="result"></ResultComponent>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import InputComponent from "./InputComponent.vue";
import ResultComponent from "./ResultComponent.vue";

export default {
  name: "RefinancingCalc",
  components: {
    InputComponent,
    ResultComponent
  },
  data() {
    return {
      msg: "Refinancing Calculator",
      result: {
        termYear: null,
        interestRate: null,
        estClosingCost: null,
        mortgageBalance: null,
        monthlyPayment1: null,
        monthlyPayment2: null,
        monthlyReducedBy: null,
        yearlySavings: null
      },
      visibleTab: 0
    };
  },

  methods: {
    displayResults(newMortageInfo) {
      this.result.termYear = newMortageInfo.termInYears;
      this.result.interestRate = newMortageInfo.interestRate;
      this.result.estClosingCost = newMortageInfo.estClosingCost;
      this.result.mortgageBalance = newMortageInfo.mortgageBalance;
      this.result.monthlyPayment1 = newMortageInfo.monthlyPayment1;
      this.result.monthlyPayment2 = newMortageInfo.monthlyPayment2;
      this.result.monthlyReducedBy = newMortageInfo.monthlyReducedBy;
      this.result.yearlySavings = newMortageInfo.yearlySavings;
      this.visibleTab = 1;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>
