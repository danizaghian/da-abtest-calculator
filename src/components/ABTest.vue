<template>
  <div class="ab-test">
    <div class="p-5 mb-4 bg-light rounded-3 text-center">
      <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold">A/B Test Size Sample Calculator</h1>
        <p class="fs-4">Calculate the sample sizes for your experiments</p>
      </div>
    </div>
    <div class="container">
      <!-- Start Overview -->
      <div class="row mb-4">
        <h5 class="mb-3">Setting up an AB Test: What sample size do I need?</h5>
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
        <div class="col">
          <h5 class="mb-3">Baseline Conversion Rate</h5>
          <p>
            The preexisting or expected conversion rate of the control group.
            This could be a click-through ra te, retention rate, or positive
            rating rate.
          </p>
        </div>
        <div class="col">
          <h5 class="mb-3">Minimum Detectable Effect</h5>
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
        <div class="col text-center">
          <h2>{{ bcr }}</h2>
          <!-- <label for="bcr">Baseline Conversion Rate</label><br /> -->
          1
          <input
            type="range"
            id="bcr"
            name="bcr"
            min="1"
            max="99"
            v-model="bcr"
          />
          99
        </div>
        <div class="col text-center">
          <h2>{{ mde }}</h2>
          <!-- <label for="mde">Minimum Detectable Effect</label><br /> -->
          1
          <input
            type="range"
            id="mde"
            name="mde"
            min="1"
            max="50"
            v-model="mde"
          />
          50
        </div>
      </div>
      <!-- End Sliders -->
      <!-- Start Conversion Rate -->
      <div class="row mb-5">
        <p class="lead text-center text-bg-light p-4">
          We want to detect a new conversion rate &lt; 6.0% or > 10.0%
        </p>
      </div>
      <!-- End Conversion Rate -->
      <!-- Start Sizes -->
      <div class="row text-center text-bg-light p-4 border">
        <div class="col">
          <h5 class="mt-3">Test Size</h5>
          <h1>{{ testSize }}</h1>
        </div>
        <div class="col">
          <h5 class="mt-3">Control Size</h5>
          <h1>{{ controlSize }}</h1>
        </div>
      </div>
      <div class="row text-center text-bg-primary p-4 border">
        <h5 class="mt-3">Total Sample Size</h5>
        <h1>{{ testSize + controlSize }}</h1>
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
      testSize: 0,
      controlSize: 0,
    };
  },
};
</script>
