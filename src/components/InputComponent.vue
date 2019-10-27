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
            v-bind:inputField="mortgageInfo.firstMortgageBalance"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!mortgageInfo.firstMortgageBalance.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageInfo.homeVal"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!mortgageInfo.homeVal.isValid"
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
            v-bind:inputField="mortgageInfo.monthlyPayment"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageInfo.monthlyPayment.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageInfo.propertyTaxes"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageInfo.propertyTaxes.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageInfo.propertyInsurance"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageInfo.propertyInsurance.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageInfo.hoa"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageInfo.hoa.isValid"
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
                <span class="form-control">{{ totalMonthlyPayment }}</span>
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
                <option value="null" selected>-- Select Term</option>
                <option>15</option>
                <option>20</option>
                <option>30</option>
                <option>40</option>
              </select>
            </div>
          </div>
          <InputFieldComponent
            v-bind:inputField="mortgageInfo.fixedInterestRate"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageInfo.fixedInterestRate.isValid"
          ></InputFieldComponent>
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
                <button class="btn btn-primary font-weight-bold">Calculate Results</button>
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
      closingCosts: 1.5,
      newTerm: "",
      mortgageInfo: {
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
        fixedInterestRate: {
          id: "fixedInterestRate",
          value: null,
          isValid: true,
          label: "Fixed Interst Rate",
          icon: "%",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg:
            "This field is required and must be numeric and greater than zero."
        },
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
        this.mortgageInfo.firstMortgageBalance.value != null &&
        this.mortgageInfo.homeVal.value != null &&
        this.mortgageInfo.firstMortgageBalance.isValid &&
        this.mortgageInfo.homeVal.isValid
      ) {
        return this.calculateRatio(
          this.mortgageInfo.firstMortgageBalance.value,
          this.mortgageInfo.homeVal.value
        );
      } else return 105;
    },

    totalMonthlyPayment() {
      let x, y, z, annualCharges, monthlyPmt;
      x = this.mortgageInfo.propertyInsurance.value;
      y = this.mortgageInfo.propertyTaxes.value;
      z = this.mortgageInfo.hoa.value;
      monthlyPmt = this.mortgageInfo.monthlyPayment.value;
      if (this.isFieldOptional(x)) {
        x = 0;
      }
      if (this.isFieldOptional(y)) {
        y = 0;
      }
      if (this.isFieldOptional(z)) {
        z = 0;
      }
      if (this.isFieldOptional(monthlyPmt)) {
        monthlyPmt = 0;
      }
      let numbers = [x, y, z];
      annualCharges =
        numbers.reduce((total, nums) => total + parseFloat(nums), 0) / 12;

      return annualCharges + parseFloat(monthlyPmt);
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
      if (!this.isValidForm(this.mortgageInfo)) {
        event.preventDefault();
        return;
      }
      if (this.isFieldOptional(this.mortgageBalance.second.value)) {
        this.mortgageBalance.second.value = 0;
      }
      if (this.isFieldOptional(this.mortgageBalance.third.value)) {
        this.mortgageBalance.third.value = 0;
      }
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

    isValidForm(mortgageInfo) {
      let isFormValid = true;
      this.validateGreaterThanZero(mortgageInfo.homeVal);
      this.validateGreaterThanZero(mortgageInfo.firstMortgageBalance);
      this.validateGreaterThanZero(mortgageInfo.monthlyPayment);
      this.validateGreaterThanZero(mortgageInfo.fixedInterestRate);
      this.validateGreaterThanZero(mortgageInfo.propertyTaxes);
      this.validateGreaterThanZero(mortgageInfo.propertyInsurance);
      this.validateOptionalField(mortgageInfo.hoa);

      isFormValid =
        mortgageInfo.homeVal.isValid &&
        mortgageInfo.firstMortgageBalance.isValid &&
        mortgageInfo.monthlyPayment.isValid &&
        mortgageInfo.fixedInterestRate.isValid &&
        mortgageInfo.propertyTaxes.isValid &&
        mortgageInfo.propertyInsurance.isValid &&
        mortgageInfo.hoa.isValid;
      return isFormValid;
    }
  }
};
</script>

<style lang="scss" scoped>
</style>