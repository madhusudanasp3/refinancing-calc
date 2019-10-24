<template>
  <div>
    <div class="row">
      <div class="col">
        <div class="row">
          <div class="col">
            <div class="row padding-label font-weight-bold padding-left--text">
              <span>How To Use</span>
            </div>
            <div class="row padding-left--text">
              <p>The refinance calculator is provided to help you with general information regarding the possible benefits of refinancing your first mortgage. The results returned by this calculator should only be used as one of many factors in evaluating your options.</p>
              <p>This calculator returns information based on your inputs regarding your existing mortgage information. It is important that you provide accurate information in order to receive more realistic results. This calculator can only provide a general overview of your situation based on the information you provide. Your mortgage company may use different information to determine eligibility and your individual results may vary from the results shown by this calculator.</p>
            </div>
          </div>
        </div>
        <form id="form" @submit="submitForm">
          <div class="row">
            <div class="col">
              <div
                class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
              >
                <span>Your Existing Mortgage Information</span>
              </div>
            </div>
          </div>
          <InputFieldComponent
            v-bind:inputField="firstMortgageBalance"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!firstMortgageBalance.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="homeVal"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!homeVal.isValid"
          ></InputFieldComponent>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              Loan to Value (LTV) Ratio
              <br />
            </label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <span class="form-control">{{ ltvRatio }}</span>
                <div class="input-group-append">
                  <span class="input-group-text rounded-right">%</span>
                </div>
              </div>
            </div>
          </div>
          <InputFieldComponent
            v-bind:inputField="monthlyPayment"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!monthlyPayment.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="annualFees.propertyTaxes"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!annualFees.propertyTaxes.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="annualFees.propertyInsurance"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!annualFees.propertyInsurance.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="annualFees.hoa"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!annualFees.hoa.isValid"
          ></InputFieldComponent>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              Total Monthly Payment
              <br />
              <span class="desc">(Including principal, interest, taxes, insurance & HOA fees)</span>
            </label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <span class="form-control">{{ this.totalMonthlyPayment }}</span>
                <div class="input-group-append">
                  <span class="input-group-text rounded-right">%</span>
                </div>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col">
              <div
                class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
              >
                <span>Refinancing Information</span>
              </div>
            </div>
          </div>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              New Term
              <br />
              <span class="desc">(Select 15, 20, 30 or 40 years)</span>
            </label>
            <div class="col-sm-6 padding-right-none">
              <select class="form-control" v-model="newTerm" id="exampleFormControlSelect1">
                <option value="null">-- Select Term</option>
                <option>15</option>
                <option>20</option>
                <option>30</option>
                <option>40</option>
              </select>
            </div>
          </div>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              Fixed Interest Rate
              <br />
            </label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <span class="form-control">{{ this.fixedInterestRate }}</span>
                <div class="input-group-append">
                  <span class="input-group-text rounded-right">%</span>
                </div>
              </div>
            </div>
          </div>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              Closing Cost
              <br />
              <span
                class="desc"
              >(Usually 1-2% of loan amount. 1.5% has been used within this example.)</span>
            </label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <span class="form-control--plaintext font-weight-bold">{{ this.closingCosts + '%'}}</span>
              </div>
            </div>
          </div>
          <div class="form-group row">
            <div class="col">
              <div class="row">
                <button class="btn btn-secondary font-weight-bold">Calculate Results</button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import InputFieldComponent from "./InputFieldComponent";

export default {
  name: "InputComponent",
  components: {
    InputFieldComponent
  },
  data() {
    return {
      ltvRatio1: false,
      totalMonthlyPayment: 1000,
      fixedInterestRate: null,
      closingCosts: 1.5,
      newTerm: "",
      homeVal: {
        id: "estHomeValue",
        value: null,
        isValid: true,
        label: "Estimated Home Value",
        icon: "$",
        type: "text",
        placeHolder: "Enter Amount",
        errorMsg:
          "This field is required and must be numeric and greater than zero."
      },
      firstMortgageBalance: {
        id: "firstMortgageBalance",
        value: null,
        isValid: true,
        label: "First Monthly Mortgage Balance",
        icon: "$",
        type: "text",
        placeHolder: "Enter Amount",
        errorMsg:
          "This field is required and must be numeric and greater than zero."
      },
      monthlyPayment: {
        id: "monthlyPayment",
        value: null,
        isValid: true,
        label: "Monthly Payment",
        icon: "$",
        type: "text",
        placeHolder: "Enter Amount",
        errorMsg:
          "This field is required and must be numeric and greater than zero.",
        desc: "(Principal & interest only)"
      },
      annualFees: {
        propertyTaxes: {
          id: "propertyTaxes",
          value: null,
          isValid: true,
          label: "Annual Property Taxes",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg:
            "This field is required and must be numeric and greater than Zero."
        },
        propertyInsurance: {
          id: "propertyInsurance",
          value: null,
          isValid: true,
          label: "Annual Property Insurance",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg:
            "This field is optional, but if you enter it must be numeric or leave it blank."
        },
        hoa: {
          id: "hoa",
          value: null,
          isValid: true,
          label: "Annual Homeowner's Association (HOA) Fees",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount Optional",
          errorMsg:
            "This field is optional, but if you enter it must be numeric or leave it blank.",
          desc: "(If applicable)"
        }
      },

      mortBalanceLTVRatio: {
        first: null,
        second: null,
        third: null,
        totalLTVRatio: "0.0",
        totalAmountOwed: null
      }
    };
  },
  computed: {
    ltvRatio() {
      if (
        this.firstMortgageBalance.value != null &&
        this.homeVal.value != null &&
        this.firstMortgageBalance.isValid &&
        this.homeVal.isValid
      ) {
        return (this.firstMortgageBalance.value / this.homeVal.value) * 100;
      } else return 105;
    }
  },
  methods: {
    validateOptionalField(input) {
      if (this.isFieldOptional(input.value)) {
        input.isValid = true;
      } else {
        input.isValid = this.isNumeric(input.value);
      }
    },
    validateGreaterThanZero(input) {
      input.isValid = this.isValid(input.value) && parseFloat(input.value) > 0;
    },
    isFieldOptional(n) {
      return n == null || this.isEmpty(n);
    },
    isValid(n) {
      // replace with regex
      return !this.isEmpty(n) && this.isNumeric(n);
    },
    isEmpty(n) {
      return n === "";
    },
    isNumeric(n) {
      // eslint-disable-next-line no-restricted-globals
      return !isNaN(parseFloat(n)) && isFinite(n);
    },
    submitForm(event) {
      if (this.isValidForm(this.homeVal, this.firstMortgageBalance)) {
        this.validateGreaterThanZero(this.homeVal);
        this.validateGreaterThanZero(this.firstMortgageBalance);
        event.preventDefault();
        return;
      }
      if (this.isFieldOptional(this.mortgageBalance.second.value)) {
        this.mortgageBalance.second.value = 0;
      }
      if (this.isFieldOptional(this.mortgageBalance.third.value)) {
        this.mortgageBalance.third.value = 0;
      }
      this.mortBalanceLTVRatio.totalAmountOwed = this.addNumberArr([
        this.mortgageBalance.first.value,
        this.mortgageBalance.second.value,
        this.mortgageBalance.third.value
      ]);
      this.mortBalanceLTVRatio.first = this.calculateRatio(
        this.mortgageBalance.first.value,
        this.homeVal.value
      );
      this.mortBalanceLTVRatio.second = this.calculateRatio(
        this.mortgageBalance.second.value,
        this.homeVal.value
      );
      this.mortBalanceLTVRatio.third = this.calculateRatio(
        this.mortgageBalance.third.value,
        this.homeVal.value
      );

      event.preventDefault();
      this.mortBalanceLTVRatio.totalLTVRatio = this.calculateRatio(
        this.mortBalanceLTVRatio.totalAmountOwed,
        this.homeVal.value
      );

      this.$emit(
        "submit-form",
        this.mortBalanceLTVRatio,
        this.mortgageBalance,
        this.homeVal.value
      );
      event.preventDefault();
    },
    calculateRatio(x, y) {
      return parseFloat(((x / y) * 100).toFixed(2));
    },
    addNumberArr(nums) {
      let sum = 0;
      for (let num of nums) {
        sum += parseFloat(num);
      }
      return sum;
    },

    isValidForm(homeVal, firstMortgageBalance) {
      let isFormValid = true;
      isFormValid =
        !homeVal.isValid ||
        !homeVal.value ||
        !firstMortgageBalance.value ||
        !firstMortgageBalance.isValid ||
        !firstMortgageBalance.isValid ||
        !firstMortgageBalance.isValid;
      return isFormValid;
    }
  }
};
</script>

<style lang="scss" scoped>
</style>