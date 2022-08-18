<template>
  <div class="hello">
    <div>
      <h1>A/B Test Size Sample Calculator</h1>
      <p>Calculate the sample sizes for your experiments</p>
    </div>
    <div>
      <h3>Setting up an AB Test: What sample size do I need?</h3>
      <p>
        Setting up a proper A/B experiment involves selecting the appropriate
        sample size. This ensures the test is adequately powered to detect the
        change while minimizing statistical noise. In statistical terms, we want
        to detect the minimum detectable effect (MDE) with statistical
        confidence. Having too few samples produces statistically ambiguous
        results, and having too many samples can be overkill and a waste of time
        and resources.
      </p>
    </div>
    <div>
      <h3>Baseline Conversion Rate</h3>
      <p>
        The preexisting or expected conversion rate of the control group. This
        could be a click-through ra te, retention rate, or positive rating rate.
      </p>
    </div>
    <div>
      <h3>Minimum Detectable Effect</h3>
      <p>
        The <strong>relative</strong> minimum difference in conversion rate
        between the test and control group we want to observe. This can be the
        expected effect, or some minimum threshold where the experiment is not
        worth running (ie. impact is too small to care about).
      </p>
    </div>
    <div>
      <label for="bcr">Baseline Conversion Rate (between 1% and 99%):</label>
      <input type="range" id="bcr" name="bcr" min="1" max="99" v-model="bcr" />
      {{ bcr }}
    </div>
    <div>
      <label for="mde">Minimum Detectable Effect (between 1% and 50%):</label>
      <input type="range" id="mde" name="mde" min="1" max="50" v-model="mde" />
      {{ mde }}
    </div>
    <div>
      <p>We want to detect a new conversion rate &lt; 6.0% or > 10.0%</p>
    </div>
    <div>
      <h3>Test Size</h3>
      <p>{{ testSize }}</p>
      <h3>Control Size</h3>
      <p>{{ controlSize }}</p>
      <h3>Total Sample Size</h3>
      <p>{{ testSize + controlSize }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
