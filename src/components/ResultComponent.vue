<template>
  <div>
    <div id="result" class="row margin--bottom">
      <div class="col">
        <div class="row padding-label font-weight-bold padding-left--text">
          <span class>Please Note:</span>
        </div>
        <div class="row padding-p padding-left--text">
          <p>These results have been calculated based on your inputs regarding your existing mortgage information. Your mortgage company may consider additional factors in determining your eligibility and potential savings, which may result in a different payment or savings amount.</p>
          <p>Please save this information (as a PDF document) or email it to yourself, so that you may have it as a reference when you speak with your mortgage company or a housing counselor.</p>
          <i>Every situation is different. The results shown below do not include the cost of Mortgage Insurance (MI), which may be required. Contact your mortgage company to determine your exact results.</i>
          <br />
          <br />
        </div>
        <div class="row">
          <div class="col">
            <div
              class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
            >
              <span>New Mortgage Information</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">Term in Years</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ formatNumber(result.estHomeValue) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">Interest Rate</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ result.firstMortgageBalance + '%' }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            Estimated Closing Costs
            <span
              class="desc"
            >(This assumes closing costs are 1.5% of the mortgage balance and are included in the total amount financed.)</span>
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{'$ ' + formatNumber(result.secondMortgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Mortgage Balance</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.thirdMortgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            Monthly Payment
            <span class="desc">(Principal & interest only)</span>
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.totalAmountOwedToHome) }}</span>
            </div>
          </div>
        </div>
        <div
          class="form-group row background--gray background--rounded padding margin--bottom align-items-center"
        >
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            Monthly Payment
            <br />
            <span class="desc">(Principal, interest, taxes, insurance & HOA fees)</span>
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ '$' + formatNumber(result.firstLTVRatio)}}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Monthly Payment Reduced By</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ '$' + formatNumber(result.secondLTVRatio) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >You Could Potentially Save This Much Each Year On Your Payments</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ '$' + formatNumber(result.thirdLTVRatio) }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultComponent",
  props: {
    result: Object
  },
  methods: {
    formatNumber(num) {
      return Number(num).toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    }
  }
};
</script>

<style lang="scss">
</style>