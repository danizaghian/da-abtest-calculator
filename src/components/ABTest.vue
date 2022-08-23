<template>
  <div class="ab-test">
    <div class="p-4 mb-5 bg-light rounded-3 text-center">
      <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold">A/B Test Size Sample Calculator</h1>
        <p class="fs-4 mb-0">Calculate the sample sizes for your experiments</p>
      </div>
    </div>
    <div class="container" style="max-width: 960px">
      <!-- Start Overview -->
      <div class="row mb-4">
        <h5 class="mb-3 fw-bold">
          Setting up an AB Test: What sample size do I need?
        </h5>
        <p>
          Setting up a proper A/B experiment involves selecting the appropriate
          sample size. This ensures the test is adequately powered to detect the
          change while minimizing statistical noise. In statistical terms, we
          want to detect the minimum detectable effect (MDE) with statistical
          confidence. Having too few samples produces statistically ambiguous
          results, and having too many samples can be overkill and a waste of
          time and resources.
        </p>
      </div>
      <!-- End Overview -->
      <!-- Start Summaries -->
      <div class="row mb-5">
        <div class="col-xs-12 col-sm-6 mb-4">
          <h5 class="mb-3 fw-bold">Baseline Conversion Rate</h5>
          <p>
            The preexisting or expected conversion rate of the control group.
            This could be a click-through ra te, retention rate, or positive
            rating rate.
          </p>
        </div>
        <div class="col-xs-12 col-sm-6">
          <h5 class="mb-3 fw-bold">Minimum Detectable Effect</h5>
          <p>
            The <strong>relative</strong> minimum difference in conversion rate
            between the test and control group we want to observe. This can be
            the expected effect, or some minimum threshold where the experiment
            is not worth running (ie. impact is too small to care about).
          </p>
        </div>
      </div>
      <!-- End Summaries -->
      <!-- Start Sliders -->
      <div class="row mb-5">
        <div class="col-xs-12 col-sm-6 mb-4 text-center">
          <h2 class="fw-bold">{{ bcr }}%</h2>

          <input
            type="range"
            class="form-range p-4"
            id="bcr"
            min="1"
            max="99"
            v-model="bcr"
          />
        </div>
        <div class="col-xs-12 col-sm-6 mb-4 text-center">
          <h2 class="fw-bold">{{ mde }}%</h2>
          <!-- <label for="mde">Minimum Detectable Effect</label><br /> -->

          <input
            type="range"
            class="form-range p-4"
            id="mde"
            min="1"
            max="50"
            v-model="mde"
          />
        </div>
      </div>
      <!-- End Sliders -->
      <!-- Start Conversion Rate -->
      <div class="row mb-5">
        <p class="lead text-center text-bg-light p-4">
          We want to detect a new conversion rate &lt;
          <strong class="text-podium"
            >{{ calcLowerBound().toFixed(1) }}%</strong
          >
          <span v-if="calcUpperBound().toFixed(1) < 100">
            or >
            <strong class="text-podium"
              >{{ calcUpperBound().toFixed(1) }}%</strong
            >
          </span>
        </p>
      </div>
      <!-- End Conversion Rate -->
      <!-- Start Sizes -->
      {{ calcCriticalNorm(bcr / 100, calcUpperBound(), calcLowerBound()) }}
      <div class="row text-center text-bg-light p-4 border">
        <div class="col">
          <h5 class="mt-3">Test Size</h5>
          <h1>{{ formatDisplaySize(nGroup) }}</h1>
        </div>
        <div class="col">
          <h5 class="mt-3">Control Size</h5>
          <h1>{{ formatDisplaySize(nGroup) }}</h1>
        </div>
      </div>
      <div class="row text-center text-bg-podium p-4 border mb-5">
        <h5 class="mt-3">Total Sample Size</h5>
        <h1>{{ formatDisplaySize(nGroup * 2) }}</h1>
      </div>
      <!-- End Sizes -->
    </div>
  </div>
</template>

<script>
export default {
  name: "ABTest",
  props: {
    msg: String,
  },
  data() {
    return {
      bcr: 1,
      mde: 1,
      nGroup: 0,
    };
  },
  created: function () {},
  methods: {
    calcLowerBound: function () {
      let bcrPercentage = this.bcr / 100;
      let mdePercentage = this.mde / 100;
      return 100 * (bcrPercentage * (1 - mdePercentage));
    },
    calcUpperBound: function () {
      let bcrPercentage = this.bcr / 100;
      let mdePercentage = this.mde / 100;
      return 100 * (bcrPercentage * (1 + mdePercentage));
    },
    normInv: function (p) {
      // Implements Acklam’s method for approximating the inverse cumulative
      // normal distribution. See resources:
      // https://stackedboxes.org/2017/05/01/acklams-normal-quantile-function/
      // https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4630740

      // Coefficients in polynomial approximations.
      let a6 = -39.69683028665376;
      let a5 = 220.9460984245205;
      let a4 = -275.9285104469687;
      let a3 = 138.357751867269;
      let a2 = -30.66479806614716;
      let a1 = 2.506628277459239;

      let b5 = -54.47609879822406;
      let b4 = 161.5858368580409;
      let b3 = -155.6989798598866;
      let b2 = 66.80131188771972;
      let b1 = -13.28068155288572;

      let c6 = -0.007784894002430293;
      let c5 = -0.3223964580411365;
      let c4 = -2.400758277161838;
      let c3 = -2.549732539343734;
      let c2 = 4.374664141464968;
      let c1 = 2.938163982698783;

      let d4 = 0.007784695709041462;
      let d3 = 0.3224671290700398;
      let d2 = 2.445134137142996;
      let d1 = 3.754408661907416;

      let q;
      let r;
      let l;
      let m;
      let result;

      // Approximation for lower region.
      if (0 < p && p < 0.02425) {
        q = Math.sqrt(-2 * Math.log(p));
        l = ((((c6 * q + c5) * q + c4) * q + c3) * q + c2) * q + c1;
        m = (((d4 * q + d3) * q + d2) * q + d1) * q + 1;
        result = l / m;

        // Approximation for central region.
      } else if (0.02425 <= p && p <= 0.97575) {
        q = p - 0.5;
        r = q * q;
        l = (((((a6 * r + a5) * r + a4) * r + a3) * r + a2) * r + a1) * q;
        m = ((((b5 * r + b4) * r + b3) * r + b2) * r + b1) * r + 1;
        result = l / m;

        // Approximation for upper region.
      } else if (0.97575 < p && p < 1) {
        q = Math.sqrt(-2 * Math.log(1 - p));
        l = ((((c6 * q + c5) * q + c4) * q + c3) * q + c2) * q + c1;
        m = (((d4 * q + d3) * q + d2) * q + d1) * q + 1;
        result = -l / m;
      } else {
        result = 0;
      }
      return result;
    },
    calcCriticalNorm: function (bcr, ub, lb) {
      // console.log(`bcr: ${bcr}`);
      ub = ub / 100;
      lb = lb / 100;
      // console.log(`ub: ${ub}`);
      // console.log(`lb: ${lb}`);
      // ADVANCED: alpha has a toggle between 0.01 - 0.1
      let alpha = 0.05;
      // ADVANCED: beta has a toggle between 0.05 - 0.38
      let beta = 0.2;
      let zA = this.normInv(1 - alpha);
      let zB = this.normInv(beta);
      // console.log(`zA: ${zA}`);
      // console.log(`zB: ${zB}`);
      // calculate standard deviations
      let sdA = Math.sqrt(2 * bcr * (1 - bcr));
      // console.log(`sdA: ${sdA}`);
      let sdB1 = Math.sqrt(bcr * (1 - bcr) + lb * (1 - lb));
      let sdB2 = Math.sqrt(bcr * (1 - bcr) + ub * (1 - ub));
      // console.log(`sdB1: ${sdB1}`);
      // console.log(`sdB2: ${sdB2}`);
      // calculate sample size required of each group
      // lb: bcr cannot be too close to 0
      // ub: bcr cannot be too close to 0
      let nB1 = Math.pow((zA * sdA - zB * sdB1) / (lb - bcr), 2);
      // console.log(`nb1: ${nB1}`);
      let nB2;
      if (ub >= 1) {
        nB2 = 0;
      } else {
        nB2 = Math.pow((zA * sdA - zB * sdB2) / (ub - bcr), 2);
      }
      // console.log(`nB2: ${nB2}`);
      this.nGroup = Math.ceil(Math.max(nB1, nB2));
      // return this.nGroup;
    },
    formatDisplaySize: function (size) {
      // always round up: do all math first, then do rounding.
      // Digits should always be 1-3 digits total
      // 0 - 999 => display number
      // 12,532 => 12.6K
      // 1000 - 999,999 => 1.2K, 12.5K, 684K
      // 1,000,000 + => 12.2M, 24.1M
      console.log(size);
      if (size > 10000000) {
        size = `${Number(size.toPrecision(3)).toString().slice(0, 3) / 10}M`;
      } else if (size > 999999 && size < 10000000) {
        size = `${Number(size.toPrecision(3)).toString().slice(0, 2) / 10}M`;
      } else if (size > 100000 && size <= 999999) {
        size = `${Number(size.toPrecision(3)).toString().slice(0, 3)}K`;
      } else if (size > 10000 && size <= 99999) {
        size = `${Number(size.toPrecision(3)).toString().slice(0, 3) / 10}K`;
      } else if (size > 1000 && size <= 9999) {
        size = `${Number(size.toPrecision(3)).toString().slice(0, 3) / 100}K`;
      }
      return size;
    },
  },
};
</script>
