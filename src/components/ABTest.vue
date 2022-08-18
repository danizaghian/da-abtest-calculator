<template>
  <div class="ab-test">
    <div class="p-5 mb-5 bg-light rounded-3 text-center">
      <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold">A/B Test Size Sample Calculator</h1>
        <p class="fs-4">Calculate the sample sizes for your experiments</p>
      </div>
    </div>
    <div class="container">
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
        <div class="col">
          <h5 class="mb-3 fw-bold">Baseline Conversion Rate</h5>
          <p>
            The preexisting or expected conversion rate of the control group.
            This could be a click-through ra te, retention rate, or positive
            rating rate.
          </p>
        </div>
        <div class="col">
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
        <div class="col text-center">
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
        <div class="col text-center">
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
          <strong class="text-podium">6.0%</strong> or >
          <strong class="text-podium">10.0%</strong>
        </p>
      </div>
      <!-- End Conversion Rate -->
      <!-- Start Sizes -->
      <div class="row text-center text-bg-light p-4 border">
        <div class="col">
          <h5 class="mt-3">Test Size</h5>
          <h1>{{ testSize }}K</h1>
        </div>
        <div class="col">
          <h5 class="mt-3">Control Size</h5>
          <h1>{{ controlSize }}K</h1>
        </div>
      </div>
      <div class="row text-center text-bg-podium p-4 border mb-5">
        <h5 class="mt-3">Total Sample Size</h5>
        <h1>{{ testSize + controlSize }}K</h1>
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
